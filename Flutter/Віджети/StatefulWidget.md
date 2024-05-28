>`StatefulWidget` використовується для створення віджетів, які можуть змінювати свій стан під час роботи програми. Це дозволяє створювати динамічні інтерфейси, які можуть реагувати на дії користувача або інші події.

#### Основні характеристики:
- Має стан, який може змінюватися з часом.
- Складається з двох класів: сам `StatefulWidget` і супутній клас `State`, який зберігає змінний стан.
- Може перебудовуватися в залежності від змін у стані.
#### Структура:
- **StatefulWidget**: Клас, який є незмінним і створює екземпляр свого супутнього `State`.
- **State**: Клас, який зберігає стан і визначає метод `build`, який повертає віджет.

```dart
class MyStatefulWidget extends StatefulWidget {
  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Stateful Widget Example'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text('You have pushed the button this many times:'),
            Text('$_counter', style: Theme.of(context).textTheme.headline4),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
```