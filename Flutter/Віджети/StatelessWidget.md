>`StatelessWidget` використовується для створення віджетів, які не змінюють свого стану після їх створення. Це означає, що такі віджети не можуть змінюватися під час роботи програми. Всі дані, необхідні для побудови інтерфейсу, передаються через конструктор.
#### Основні характеристики:
- Не має стану, який змінюється.
- Перебудовується лише тоді, коли змінюються зовнішні параметри.
- Простіший і більш ефективний у порівнянні з `StatefulWidget`.

```dart
class MyStatelessWidget extends StatelessWidget {
  final String title;

  MyStatelessWidget({required this.title});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Center(
        child: Text('This is a stateless widget'),
      ),
    );
  }
}

```