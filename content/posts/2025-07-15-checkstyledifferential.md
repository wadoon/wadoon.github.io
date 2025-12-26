---
title: "Differential Checkstyle"
date: 2025-09-27
---

Someone deactivated Sonarqube. We seem to have a deeper inner confusion with our common understanding of clean code. **This is my last attempt at getting something going,** after this I am on my wit's end. 

First, this PR reactivates the Checkstyle using updated versions of the `GitDiffFilter`, which are now externalized as Maven dependencies. The code of the diff filter are here: https://github.com/wadoon/checkstyle-key-extensions. Ownership transferred on acceptance, also published on Maven Central. 

The address of the diff is 
```gradle
checkstyle("org.key-project.devel:checkstyle-key-extensions:1.17-SNAPSHOT")
```
which can then be used across projects. The `checkstyle-key-extensions` are updated to the latest version of checkstyle, which has two filters now: one for files before parsing, one for issues after linting. `checkstyle-key-extensions` uses both. 

Checkstyle is invoked via Gradle via `checkstyleMain` and `checkstyleTest`. Dependencies are managed by Gradle. 


The bottleneck is the limited reported possibilities on Github. This PR tries to use everything.

1. A Markdown Job summary is added
[![image](https://github.com/user-attachments/assets/2011901c-c3e2-4069-a154-06058b13d79c)](https://github.com/KeYProject/key/actions/runs/12911732379?pr=3539#summary-3600483672)
2. [Sarif files are joined and uploaded:](https://github.com/KeYProject/key/security/code-scanning?query=pr%3A3539+is%3Aopen)
      ![image](https://github.com/user-attachments/assets/3cd5b0b9-247c-417d-8a77-892be9a81c3c)
3. HTML checkstyle artifacts can be downloaded
      ![image](https://github.com/user-attachments/assets/6701a1a3-5e70-4e40-8351-d0c3e3f117e8)
4. Moreover, it simplifies the Jacoco integration. 
5. At the end of the Github tests runs, a Gradle task reads JUnit test xml reports and prints out [Github annotations](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/workflow-commands-for-github-actions#setting-an-error-message). Failing tests should appear on the Github workflow summary. 
   
   <img width="1498" height="243" alt="image" src="https://github.com/user-attachments/assets/9983b1a7-1eb0-42a4-b4a5-9921768899fb" />



**It does not** add a comment to the PR as this is not reliable due to reduced permissions on workflow executions for foreign repositories. 


## Testing 

Testing of this PR is hard. You need 

1. failing test cases
2. checkstyle violations

It might be a good choice to merge this branch squashily on some of your working branches for testing. 
