---
title: "Gradle Test Logging for Github Workflow"
date: 2025-12-26
---

You can use the `TestListener` interface of Gradle Test tasks to get a proper
clutter-free printing of tests.

The following listener is only activated on if `CI=true` is set on the
environment. Moreover, you can also couple it with the environment variable
`RUNNER_DEBUG` to activate this logging only on debugging runs.

```kotlin
 if ("true" == System.getenv("CI")) {
        addTestListener(object : TestListener {
            override fun afterTest(testDescriptor: TestDescriptor, result: TestResult) {
                if (result.resultType == TestResult.ResultType.FAILURE) {
                    val message = "${testDescriptor.className}#${testDescriptor.name}; Error: `${result.exception}`"
                    println("::error title=${testDescriptor.displayName}::$message")
                }
            }

            override fun afterSuite(suite: TestDescriptor, result: TestResult) {
                println("::endgroup::")
            }
            override fun beforeSuite(suite: TestDescriptor) {
                println("::group::${suite.displayName}")
            }

            override fun beforeTest(testDescriptor: TestDescriptor) {}
        })
}
```
