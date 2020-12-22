# ktfmt-gradle 🧹🐘

A wrapper to apply [ktfmt](https://github.com/facebookincubator/ktfmt) to your Gradle builds, and reformat you Kotlin
source code like a glimpse.

[![Plugin Portal](https://img.shields.io/maven-metadata/v/https/plugins.gradle.org/m2/com/ncorti/ktfmt/gradle/com.ncorti.ktfmt.gradle.gradle.plugin/maven-metadata.xml.svg?label=Gradle%20Plugin%20Portal&colorB=brightgreen&logo=gradle)](https://plugins.gradle.org/plugin/com.ncorti.ktfmt.gradle) [![Pre Merge Checks](https://github.com/cortinico/ktfmt-gradle/workflows/Pre%20Merge%20Checks/badge.svg)](https://github.com/cortinico/ktfmt-gradle/actions?query=workflow%3A%22Pre+Merge+Checks%22) ![Language](https://img.shields.io/github/languages/top/cortinico/kotlin-android-template?color=blue&logo=kotlin) [![License](https://img.shields.io/github/license/cortinico/kotlin-android-template.svg)](LICENSE) [![Use this template](https://img.shields.io/badge/from-kotlin--gradle--plugin--template-brightgreen?logo=dropbox)](https://github.com/cortinico/kotlin-gradle-plugin-template/generate) [![Twitter](https://img.shields.io/badge/Twitter-@cortinico-blue.svg?style=flat&logo=twitter)](http://twitter.com/cortinico)

## How to use 👣

**ktfmt-gradle** is distributed through [Gradle Plugin Portal](https://plugins.gradle.org/). To use it you need to add
the following dependency to your gradle files.

Please note that those code needs to be added the gradle file of the **module** where you want to reformat the code (**
not the top level** build.gradle[.kts] file).

If you're using the `plugin{}` blocks in your Gradle file:

```kotlin
plugins {
    id("com.ncorti.ktfmt.gradle") version "<latest_version>"
}
```

If you're instead using the Groovy Gradle files and the old `buildscript` block:

```groovy
buildscript {
    repositories {
        maven { url "https://plugins.gradle.org/m2/" }
    }

    dependencies {
        classpath "com.ncorti.ktfmt.gradle:plugin:<latest_version>"
    }
}

apply plugin: "com.ncorti.ktfmt.gradle"
```

### Task

By default, `ktfmt-gradle` will add two Gradle tasks to your build:

- `ktfmtCheck` that will check if the code in your module is ktfmt-compliant
- `ktfmtFormat` that will reformat your code with ktfmt

Those two tasks will invoke `ktfmt` on the **whole module**. More specific tasks are avialable based on the module type.

#### Jvm/Js Modules

For Jvm/Js modules, the plugin will create a check/format task for **every source set**. For example, jvm modules will
have a `ktfmtCheckMain` and `ktfmtCheckTest` tasks for the `main` and `test` source sets.

#### Multiplatform Modules

Kotlin Multiplatform modules will have separate tasks for **every target/source set**. You will have tasks
like `ktfmtCheckCommonMain` and `ktfmtCheckCommonTest` and so on. If you target also Android, the tasks explained below
will be added as well.

#### Android Modules

Kotlin Android modules will also have separate tasks for **every source set**. Due to how source sets are handled on
Android, you can expect ktfmt tasks to follow the convention: `ktfmt[Check|Format][SourceSet][Variant]JavaSource`. For
example, the `ktfmtCheckAndroidTestDebugJavaSource`.

## Features 🎨

- 100% Kotlin-only plugin.
- **Parallel** file processing with Kotlin Coroutines.
- Supports **incremental** builds (i.e. checks tasks won't rerun if source is unchanged).
- Configurable thanks to the `ktfmt{}` block.
- Integrated with Jvm/Android/KMM modules.

The minimum supported runtime version is **JDK 11**, released September 2018.

## Configuring 🛠

You can configure the behavior of the `ktfmt` invocation with the `ktfmt` block in your `build.gradle.[kts]` file.

To enable **dropbox style** (4 spaces indentation) you should simply:

```kotlin
ktfmt {
    dropboxStyle()
}
```

If you wish to have further control on the tool you can instead:

```kotlin
ktfmt {
    // Breaks lines longer than maxWidth. Default 100.
    maxWidth.set(80)
    // blockIndent is the indent size used when a new block is opened, in spaces.
    blockIndent.set(8)
    // continuationIndent is the indent size used when a line is broken because it's too
    continuationIndent.set(8)
    // Whether ktfmt should remove imports that are not used.
    removeUnusedImports.set(false)
}
```

## Contributing 🤝

Feel free to open a issue or submit a pull request for any bugs/improvements.

## License 📄

This project is licensed under the MIT License - see the [License](License) file for details