# Kotlin Tips 🚀

[![License](https://img.shields.io/badge/license-Apache%202.0-green.svg)](https://github.com/lordcodes/lordcodes-dev-tips/blob/master/LICENSE)
[![@lordcodes](https://img.shields.io/badge/contact-@lordcodes-blue.svg?style=flat)](https://twitter.com/lordcodes)
[![Lord Codes Blog](https://img.shields.io/badge/blog-Lord%20Codes-yellow.svg?style=flat)](https://www.lordcodes.com)

If you like what I am sharing, please don't hesitate to follow me [on Twitter](https://twitter.com/lordcodes) and check out [my blog](https://www.lordcodes.com).

You can also check out my [Swift tips](swift-tips.md).

## Kotlin Table of Contents

[#4 Set up Kotlin sources in src/main/kotlin from Gradle Kotlin DSL](#4-set-up-kotlin-sources-in-srcmainkotlin-from-gradle-kotlin-dsl)

[#3 Make the ktlint Gradle task incremental](#3-make-the-ktlint-gradle-task-incremental)

[#2 Enable Java 8 compatibility for Kotlin sources](#2-enable-java-8-compatibility-for-kotlin-sources)

[#1 Manage Gradle dependencies using Kotlin code in buildSrc](#1-manage-gradle-dependencies-using-kotlin-code-in-buildsrc)

## Kotlin Tip List

### [#4 Set up Kotlin sources in src/main/kotlin from Gradle Kotlin DSL](https://twitter.com/lordcodes/status/1087031040009531394)

[Twitter](https://twitter.com/lordcodes/status/1087031040009531394)

When converting my Android Gradle files to Kotlin, one part that wasn't clear was how to keep sources within src/main/kotlin instead of java. It turns out you just need to get the source-set by name and make `srcDirs` a function call. Works like a dream! 🛠

```kotlin
sourceSets {
    getByName("main").java.srcDirs("src/main/kotlin")
    getByName("test").java.srcDirs("src/test/kotlin")
    getByName("androidTest").java.srcDirs("src/androidTest/kotlin")
}
```

[Return to top](#kotlin-tips-)

### [#3 Make the ktlint Gradle task incremental](https://twitter.com/lordcodes/status/1068119144141328384)

[Twitter](https://twitter.com/lordcodes/status/1068119144141328384)

When setting up ktlint to lint your Kotlin code, you can make the main Gradle task incremental by specifying the inputs and outputs. Really handy to squeeze out that extra bit of performance! 🚄

```groovy
// Specify the input files and output location
def inputFiles = fileTree(dir: "src", include: "**/*.kt")  
def outputDir = "${buildDir}/reports/ktlint/"  
  
task ktlint(type: JavaExec, group: "verification") {
  // Set the inputs and outputs on the task
  inputs.files(inputFiles)  
  outputs.dir(outputDir)  
  
  // Configure ktlint as normal
  description = "Check Kotlin code style."  
  classpath = configurations.ktlint  
  main = "com.github.shyiko.ktlint.Main"  
  args "src/**/*.kt", "--reporter=plain", 
    "--reporter=checkstyle,output=${outputDir}ktlint-checkstyle.xml"  
}
```

[Return to top](#kotlin-tips-)

### [#2 Enable Java 8 compatibility for Kotlin sources](https://twitter.com/lordcodes/status/1067822936093007872)

[Twitter](https://twitter.com/lordcodes/status/1067822936093007872)

Enabling Java 8 compatibility via the compileOptions block only works for Java sources. To do the same for Kotlin you need to use kotlinOptions on any KotlinCompile Gradle tasks. Interesting… 🤔

```groovy
// For Kotlin
tasks.withType(org.jetbrains.kotlin.gradle.tasks.KotlinCompile).all {  
    kotlinOptions {  
        jvmTarget = JavaVersion.VERSION_1_8  
    }  
}

// For Java
android {
  compileOptions {  
    sourceCompatibility JavaVersion.VERSION_1_8  
    targetCompatibility JavaVersion.VERSION_1_8  
  }
}
```

[Return to top](#kotlin-tips-)

### [#1 Manage Gradle dependencies using Kotlin code in buildSrc](https://twitter.com/lordcodes/status/1067417520393601024)

[Twitter](https://twitter.com/lordcodes/status/1067417520393601024)

Using Kotlin to maintain your Gradle dependencies within buildSrc allows you to use a familiar syntax, gives you auto-complete within Groovy or Kotlin Gradle scripts and allows you to click through to a particular dependencies definition! Sweet 🍭

**→ /buildSrc/src/main/java/com/myapp/Versions.kt**
```kotlin
package com.myapp

object Versions {
  object AndroidX {
    const val APPCOMPAT = "1.0.2"
  }
  const val KOTLIN = "1.3.10"
  ...
}
```

**→ /buildSrc/src/main/java/com/myapp/Libs.kt**
```kotlin
package com.myapp

object Libs {
  object AndroidX {
    const val APPCOMPAT = "androidx.appcompat:appcompat:${Versions.AndroidX.APPCOMPAT}"
  }
  const val KOTLIN_STDLIB = "org.jetbrains.kotlin:kotlin-stdlib:${Versions.KOTLIN}" 
}
```

**→ /src/app/build.gradle**
```groovy
import com.myapp.Libs

dependencies {
  implementation Libs.KOTLIN_STDLIB
  implementation Libs.AndroidX.APPCOMPAT
}
```

[Return to top](#kotlin-tips-)

## Contributing

If you notice any typing mistakes or errors in the code snippets, then please feel free to let me know with a PR.
