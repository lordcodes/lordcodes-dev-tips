# Kotlin Tips 🚀

[![License](https://img.shields.io/badge/license-Apache%202.0-green.svg)](https://github.com/lordcodes/lordcodes-dev-tips/blob/master/LICENSE)
[![@lordcodes](https://img.shields.io/badge/contact-@lordcodes-blue.svg?style=flat)](https://twitter.com/lordcodes)
[![Lord Codes Blog](https://img.shields.io/badge/blog-Lord%20Codes-yellow.svg?style=flat)](https://www.lordcodes.com)

If you like what I am sharing, please don't hesitate to follow me [on Twitter](https://twitter.com/lordcodes) and check out [my blog](https://www.lordcodes.com).

You can also check out my [Swift tips](swift-tips.md).

## Kotlin Table of Contents

[#1 Manage Gradle dependencies using Kotlin code in buildSrc](#1-manage-gradle-dependencies-using-kotlin-code-in-buildsrc)

## Kotlin Tip List

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
