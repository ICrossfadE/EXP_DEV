1. Створюємо фаіл `Назва_block.dart`.
2. Створюємо звичайний `class`  та наслідуємо його від `Block<>`.
3. В `super` передаємо початковий `state`.
   ```dart
class CounterBloc extends Bloc<> {

// constructor
  CounterBloc() : super(0)
  }
```
	Створюємо Івенти.
   ```dart
class CounterBloc extends Bloc<> {

// constructor
  CounterBloc() : super(0)
}

// Events
class CounterIncrementEvent{}
class CounterDecrementEvent{}
```
	Для того щоб мати змогу використовувати два івента потрібно об'єднати їх `abstract` класом. Та поширити їх до цього класу.
   ```dart
class CounterBloc extends Bloc<> {}

// constructor
  CounterBloc() : super(0)
// Events
abstract class CounterEvents {}
class CounterIncrementEvent extends CounterEvents {}
class CounterDecrementEvent extends CounterEvents {}
``` 
	В Block передати дженериком абстракний клас і `state` тут для прикладу буде `int`.
   ```dart
class CounterBloc extends Bloc<CounterEvents, int> {}

// constructor
  CounterBloc() : super(0)
// Events
abstract class CounterEvents {}
class CounterIncrementEvent extends CounterEvents {}
class CounterDecrementEvent extends CounterEvents {}
``` 
	Декларуємо в конструкторі майбутні івенти. Для цього нам потрібно використовувати метод `on`
	- `on` приймає в себе дженерик `event` на який він буде реагувати і також нам потрібно передати першим параметром колбек функцію.
		`on<Event>(callback) `
	- `callback` функція має прийняти в себе написаний івент та `event` для того щоб мати доступ до значень які ми зможемо передавати з допомогою написаного класу зараз це буде `CounterIncrementEvent`.
	- Також треба передати слідуючи аргументом `Emittrer` який дженериком приймає `state` і функцію яка буде змінювати наш `state` функція `emit`.
   ```dart
class CounterBlocc extends Bloc<CounterEvents, int> {}
class CounterBlocc extends Bloc<CounterEvents, int> {

  // constructor
  CounterBlocc() : super(0) {
    on<CounterIncrementEvent>(_onIncrement);
    on<CounterDecrementEvent>(_onDecrement);
  }
  
  void _onIncrement(CounterIncrementEvent event, Emitter<int> emit) {}
  void _onDecrement(CounterDecrementEvent event, Emitter<int> emit) {}
}
// Events
abstract class CounterEvents {}
class CounterIncrementEvent extends CounterEvents {}
class CounterDecrementEvent extends CounterEvents {}
``` 
	*Висновок*
	- `on` це функція яка реагує на <`event`> який передаємо в дженерик і вона приймає в себе `collback` функцію яка містить в собі доступ до цього
	  `CounterIncrementEvent event` і другим параметром `Emitter` який дозволяє змінити `state`
---
	Для того щоб змінити стан нам потрібно викликати функцію `emit()`.
   ```dart
   void _onIncrement(CounterIncrementEvent event, Emitter<int> emit) {
    emit(state + 1);
  }
  void _onDecrement(CounterDecrementEvent event, Emitter<int> emit) {
    emit(state - 1);
  }
```

Дальше щоб використати наш `state` з нашими івентами потробно обгорнути потрібний віджет `BlockProvider()`
```dart
BlocProvider<CounterBloc>(
	create: (context) => CounterBlock()
)
```
- Провайдер приймає дженериком наш `bloc - CounterBloc`. 
- Властивість `create:`  ми передаємо функція яка нам створить наш `bloc - CounterBloc` і поверне його `BlockProvider`.
- Після цього всі `child` віджета будуть мати доступ до стану та івентів.
  ```dart
   Widget build(BuildContext context) {
    return BlocProvider<CounterBloc>(
      create: (context) => CounterBloc(),
      child: BlocBuilder<CounterBloc, int>(
        builder: (context, state) {
          return Scaffold()
          }
         );
        )
       };
```
	- Для рендеру стану нам потрібно обгорнути потрібний віджет в `BlockBuilder`.
	- Дженериком також передаємо наш `bloc - CounterBloc`і другим аргументом наш `state`
	- Для виводу стейту `this.state` в потрібному місці.
	- Дальше для використання наших івентів потрібно у віджета який має властивість `inPressed:` передати `BlocProvider`.
  ```dart
  IconButton(
	  onPressed: () {
		BlocProvider.of<CounterBloc>(context).add(CounterIncrementEvent());
	  },
	  icon: const Icon(Icons.add),
	),
```
	Для зручності можемо цей вираз зберегти в змінну після `BlockBuilder`.
```dart
Widget build(BuildContext context) {
    return BlocProvider<CounterBloc>(
      create: (context) => CounterBloc(),
      child: BlocBuilder<CounterBloc, int>(
        builder: (context, state) {
        final bloc = BlocProvider.of<CounterBloc>(context);
          return Scaffold()
          }
         );
        )
       };
```

   ```dart
IconButton(
	  onPressed: () {
		bloc.add(CounterIncrementEvent());
	  },
	  icon: const Icon(Icons.add),
	),
```

Також після загрузки нашого додатку можна зразу зробити якусь дію через оператор `..`
```dart
Widget build(BuildContext context) {
    return BlocProvider<CounterBloc>(
      create: (context) => CounterBloc()..add(event),
      child: BlocBuilder<CounterBloc, int>(
        builder: (context, state) {
        final bloc = BlocProvider.of<CounterBloc>(context);
          return Scaffold()
          }
         );
        )
       };
```
Таким чином ми після створення цього елементу зразу бeде застосований `event` який буде в `CounterBloc`.