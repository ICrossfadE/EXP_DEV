
## Вимикання стилів у кнопки
```dart
style: ButtonStyle(
	overlayColor: WidgetStateProperty.all(Colors.transparent),
	splashFactory: NoSplash.splashFactory,
),
```

- overlayColor - відповідає за колір при натисканні на кнопку.
- splashFactory - відповідає за анімацію при натисканні. 
---
## Обнулення падингів в кнопці
```dart
padding: WidgetStateProperty.all(const EdgeInsets.all(0)),
```