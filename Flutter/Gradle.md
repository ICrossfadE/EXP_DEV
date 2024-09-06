## Як синхронізувати файли gradle.

Потрібно  відкрити термінал на рівні `gradlew`
![[Pasted image 20240730171216.png]]
- Після цього запускаємо команду `./gradlew clean build`
- Далі оновлюємо версію `gradle`
```dart
buildscript {
    ext.kotlin_version = '1.9.0' // Виберіть актуальну версію Kotlin
    repositories {
        google()
        mavenCentral()
    }

    dependencies {
        classpath "com.android.tools.build:gradle:8.0.0" //відповідну версію
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}
```
- Додаємо в `buildscript` подібний код
- Синхронізуємо файли `gradle` командою `./gradlew build` 