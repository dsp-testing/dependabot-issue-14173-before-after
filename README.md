# Dependabot issue 14173: before and after

Controlled reproduction for
[dependabot-updates#14173](https://github.com/github/dependabot-updates/issues/14173).

The two states use identical dependency inputs. The only after-state change is
a resolution rule on `buildscript.configurations.classpath`.

## Before

The `before-buildscript-fix` branch demonstrates that Gradle has two separate
dependency containers:

```text
./gradlew buildEnvironment
  jackson-databind:2.21.4

./gradlew dependencies --configuration runtimeClasspath
  jackson-databind:2.9.10 -> 2.22.1
```

The project configuration is safe because `settings.gradle` substitutes Jackson
to `2.22.1`. The buildscript classpath remains vulnerable at `2.21.4`.

## After

The `after-buildscript-fix` branch adds only a buildscript classpath resolution
rule. Both containers then resolve Jackson to `2.22.1`.

The dependency-submission workflow records all Gradle output as a workflow
artifact before submitting the resolved graph to GitHub.
