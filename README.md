# Patient Portal for Android

---

## About

This project is a Patient Portal for the Android platform.

---

## Requirements

- Android SDK

---

## Project Dependencies

*to be determined...*

---

## Branching Strategy
The project uses a [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) branching strategy.
 - Feature branches are cut from Develop and merged back in to Develop.
 - Release branches are cut from Develop and merged in to Develop and Master

![Feature Branching](images/release-branches.png)

---

## Release Process
While this process is currently done manually, automation is coming soon...

#### Release Branch and Tagging
1. Cut release branch from Develop branch
2. Deploy a build to Firebase from the release branch.
4. Open pull request from release branch to Master ***and*** Develop
5. Merge to Master ***and*** Develop
6. Create a Release tag from Master
7. Release to Google Play Store

## Setup Githook
When adding a commit message it is helpful to reference the JIRA ticket for which the code is being committed. That way it is easy for any future developer (including your future self) to access more information on that commit. If I fix a bug and use the commit message:
Fixed the advertisement bug

Not only is the message itself not helpful but also there is nothing to even point me in the right direction.

If I fix the same bug and use the commit message:
This both clearly states what was done as well as gives me a link from BitBucket directly to the JIRA ticket.

"But I'm lazy"

Me too! Thankfully we can use Git Hooks to automatically add branch information to our commits, freeing us up to put more thought and energy into writing a good commit message. With a little bit of work up front you don't even have to think about it anymore.

1. ``` ./gradlew installGitHookPreCommit ```
2. ``` ./gradlew installGitHookPreCommitMsg ```

#### Now fun part. When you do "git commit"

 - Automatically adds JIRA ID to every commit message.
 - Script pulls JIRA ticket number from branch name
 - Branch Names Suppoted : [origin/feature/CHAPP-123-some-feature] or [fearure/CHAPP-123-some-feature] or [CHAPP-123-some-feature]
 - Commit Message : "this is my commit message" (you do not required to put JIRA id in message)
 - Final output posted to commit will be "CHAPP-123 this is my commit message"
 - This works with terminal , Source Tree, Android Studio

That's it, you are done. Now all of your commit messages will have your branch name prepended to them. Happy coding!

## Code Quality Check
### We are using this code quality plugin https://github.com/vanniktech/gradle-code-quality-tools-plugin

![Feature Comparision](images/lint_detekt_ktlint.png)

Here I'll give a bit more information about how each of the tools will be applied. If there's a Gradle task that this plugin will generate it will also be hooked up into the `check` Gradle task. This means that when you execute `check` all of the tools will be running for you.

### Error Prone

It'll apply the [Error Prone Gradle Plugin](https://github.com/tbroyer/gradle-errorprone-plugin) which will run together with `assemble`. There's no report generated for this but you'll get compile warnings & errors.

### Lint

This will only work when one of the Android Plugins (`com.android.application`, `com.android.library`, etc.) are applied. The configuration properties of `codeQualityTools -> lint` mirror the [properties from the lintOptions](https://google.github.io/android-gradle-dsl/current/com.android.build.gradle.internal.dsl.LintOptions.html).

### Detekt

It'll use the specified detekt version and generate the `detektCheck` task which will run detekt on your code base.

### Ktlint

It'll use the specified ktlint version and then generate two tasks. `ktlint` which will run ktlint over your code and flag issues. `ktlintFormat` will reformat your code.

Below commands can be run individually

```
./gradlew assemble (Error Prone)

./gradlew lint

./gradlew detekCheck

./gradlew ktlint

./gradlew ktlintFormat

./gradlew check (To run all the above commands)

```