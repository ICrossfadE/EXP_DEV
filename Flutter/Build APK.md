навіть дуже простий додаток Flutter, що використовує зазвичай буде займати приблизно 10-15MB. Це через саму Flutter SDK, Яка включає в себе `runtime` бібліотеки і графічні ресурси.

Використання команд:

```dart
flutter build apk --release
```
- Створить `.apk` для всіх архітектур. Також об'єм пам'яті буде дуже великим `~40mb` без медіа файлів.

```dart
flutter build apk --release --target-platform=android-arm

flutter build apk --release --target-platform=android-arm64

flutter build apk --release --target-platform=android-x64
```
- Кожна команда створить окремий `.apk` для конкретних архітектур. Об'єм пам'яті буде `~18mb` без медіа файлів.