Користувацькі анімації, які ви контролюєте за допомогою AnimationController.

Для початку віджету потрібно додати `mixin` - `SingleTickerProviderStateMixin`.
- Цей `mixin` забезпечує `vsync` для `AnimationController`, що оптимізує використання ресурсів.

```dart
//Оголошуємо через late ініціалізація буде в initState
late AnimationController controller;

  @override
  void initState() {
    super.initState();
    controller = AnimationController(
      vsync: this,
      duration: const Duration(seconds: 3),
      // upperBound: 100,
    );
    controller.forward();
    controller.addListener(() {
      setState(() {
        controller.value;
      });
    });
  }

  void despose() {
    controller.dispose();
    super.dispose();
  }
```
- `controller.forward();` - Це запускає анімацію вперед.
- `controller.addListener` - Цей слухач викликає `setState` при кожній зміні значення анімації, змушуючи віджет перебудовуватися.
- `controller.value` - можна використати у віджетах для анімації елементів.
- `despose` - Це важливий крок для звільнення ресурсів, коли віджет видаляється.
