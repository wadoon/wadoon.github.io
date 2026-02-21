---
title: Suppression of Log Message on Non-Failing Tests
date: 2026-02-18T21:01:00+01:00
tags: [junit,logback] 
---

Logging is important, but annoying in cases when you do not need it. On test cases, logging helps on repairs based on failing test cases. For non-failing (negative) test cases, logging messages should be suppressed, but for failing test cases it should be printed. 

With JUnit, you can achieve this as follows: Using the *JUnit Extensions* interface. On each test execution, you disable the logging and enables and re-enable it after test execution. If the test execution was positive (aka. the test came back with an exception), you print the logging messages captured during the execution. 

In *logback*, you can capture the the logging messages with the `StringListAppender` which is hang into the root logger.  It stores the messages in a list of strings. The old (console) appender should be saved and remove from the root logger. 

You can install the lifecycle methods globally using (a) `junit-platform.properties` and (b) the `ServiceLoader` interface. [See](https://www.baeldung.com/java-junit5-run-before-all-classes#bd-implementing-global-setup-with-junit-extensions).

### `junit-platform.properties` 

```properties
junit.jupiter.extensions.autodetection.enabled=true
```

### `META-INF/services/org.junit.jupiter.api.extension.Extension`
```
de.uka.ilkd.key.testfixtures.TestLogMgr
```

### `de.uka.ilkd.key.testfixtures.TestLogMgr` 

```java
/* This file is part of KeY - https://key-project.org
 * KeY is licensed under the GNU General Public License Version 2
 * SPDX-License-Identifier: GPL-2.0-only */
package de.uka.ilkd.key.testfixtures;

import ch.qos.logback.classic.spi.ILoggingEvent;
import ch.qos.logback.core.ConsoleAppender;
import ch.qos.logback.core.encoder.LayoutWrappingEncoder;
import ch.qos.logback.core.testUtil.StringListAppender;
import org.junit.jupiter.api.extension.AfterTestExecutionCallback;
import org.junit.jupiter.api.extension.BeforeTestExecutionCallback;
import org.junit.jupiter.api.extension.ExtensionContext;
import org.slf4j.LoggerFactory;

/**
 * Extension of JUnit 5 that suppress logging of non-failing tests.
 *
 * @author Alexander Weigl
 * @version 1 (2/17/26)
 */
public class TestLogMgr implements AfterTestExecutionCallback, BeforeTestExecutionCallback {
    private static ch.qos.logback.classic.Logger root =
        (ch.qos.logback.classic.Logger) LoggerFactory
                .getLogger(org.slf4j.Logger.ROOT_LOGGER_NAME);

    private static final ConsoleAppender<ILoggingEvent> consoleAppender;

    private static final StringListAppender<ILoggingEvent> listAppender =
        new StringListAppender<>();

    static {
        consoleAppender = (ConsoleAppender<ILoggingEvent>) root.getAppender("STDOUT");
        listAppender.setContext(consoleAppender.getContext());
        listAppender.setLayout(
            ((LayoutWrappingEncoder<ILoggingEvent>) consoleAppender.getEncoder()).getLayout());
        listAppender.start();
    }

    @Override
    public void afterTestExecution(ExtensionContext context) {
        root.detachAppender(listAppender);
        root.addAppender(consoleAppender);
        if (context.getExecutionException().isPresent()) {
            for (var s : listAppender.strList) {
                System.out.print(s);
            }
            System.out.flush();
        }
        listAppender.strList.clear();
    }

    @Override
    public void beforeTestExecution(ExtensionContext context) {
        System.out.flush();
        root.detachAppender(consoleAppender);
        root.addAppender(listAppender);
    }
}
```

This based on the initial configuration of `logback.xml`:

```xml
<configuration>
    <statusListener class="ch.qos.logback.core.status.NopStatusListener" />
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <file>key.log</file>
        <append>false</append>
        <encoder>
            <pattern>%-10relative %-5level %-15thread %-25logger{5} %msg %ex%n</pattern>
        </encoder>
    </appender>

    <root level="DEBUG">
        <appender-ref ref="STDOUT"/>
    </root>
</configuration>
```

You can setup this once in the `textFixtures` using Gradles plugin `java-test-fixtures`. 