# IssueTracker #262588970

Reproducer project for issue https://issuetracker.google.com/issues/262588970

1. Compile the project
   ```bash
   gradlew assembleDebug
   ```
2. Check [`app/src/main/res/values/strings.xml`](app/src/main/res/values/strings.xml) and [`app/src/main/java/com/example/issuetracker_262588970/App.kt`](app/src/main/java/com/example/issuetracker_262588970/App.kt) and uncomment the parts you want to test.
3. Check if the IDE detects the error by itself and also run `gradlew lintDebug` to check if lint does as well. 
4. Finally, swap the dependency in [`app/build.gradle`](app/build.gradle) from `implementation(project(":lib"))` to `implementation(files(aar))`, re-build and re-check the files.

Here are the results (✔ means the error is correctly detected, ❌ means it's not):

- `implementation(project(":lib"))`
  | Usage                                                                         | CLI<br/>`lintDebug` | IDE<br/>`Electric Eel 2022.1.1` |
  |-------------------------------------------------------------------------------|---------------------|---------------------------------|
  | Reference in code<br/>`com.example.lib.R.string.lib_private_string`           | ✔                   | ❌                               |
  | Reference in XML<br/>`<string name="...">@string/lib_private_string</string>` | ❌                   | ❌                               |
  | Override in XML<br/>`<string name="lib_private_string">...</string>`          | ✔                   | ❌                               |

- `implementation(files(aar))`
  | Usage                                                                         | CLI<br/>`lintDebug` | IDE<br/>`Electric Eel 2022.1.1` |
  |-------------------------------------------------------------------------------|---------------------|---------------------------------|
  | Reference in code<br/>`com.example.lib.R.string.lib_private_string`           | ✔                   | ✔                               |
  | Reference in XML<br/>`<string name="...">@string/lib_private_string</string>` | ❌                   | ❌                               |
  | Override in XML<br/>`<string name="lib_private_string">...</string>`          | ✔                   | ✔                               |
