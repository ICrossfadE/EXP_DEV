Є три типи маршрутизатора `Anonymos Routing` `Named Routing` `Genrated Routing`.

---
## Anonymous Routing
- З назви стає розуміло що це маршрут без оголошення назви. Для такого переходу використовується:
  ```dart
  onPressed: () {
	  navigator.of(context).push(
		  MaterialPageRoute(
			  builder: (context) => SecondScreen(
				  title: 'Second Screen',
			  ),
		  ),
	),
  }
```
  Без необхідності вказувати назву маршруту.

*Якщо ми використовуємо `.push` то в такому разі сторінки будуть складатись одне на одного і щоб повернутися на початкову сторінку потрібно буде вернутись через кожну відкриту сторінку таким методом.*

Для того щоб повернутися на сторінку назад потрібно використати метод `.pop`
```dart
  onPressed: () {
	  navigator.of(context).pop();
  }```

>Така реалізація може підійти для малих проектів
---
## Named Routing
- Це маршрут з додаванням назви. Всі маршрути які будуть вводитись мають бути іменованими та мають бути встановлені в параметрі `routes` у віджеті додатку
  приклад: 

- `routes`: Приймає в себе `Map`. 
  - `key` - це буде назва шляху.
  - `value` - це буде сторінка яка буде відтворена

*Приклад з реалізацією Bloc*
  ```dart
  Widget build(BuildContext context) {
	  return MaterialApp(
		title: 'Flutter Demo',
		home: HomePage(),
		routes: {
		'/': (context) => BlocProvider.value(
			value: _counterBloc,
			child: HomePage(),
		),
		'/counter': (context) => BlocProvider.value(
			value: _counterBloc,
			child: SecondPage(),
		),
		}	  
	  )
  }
```
*Приклад без Bloc*
  ```dart
  Widget build(BuildContext context) {
	  return MaterialApp(
		title: 'Flutter Demo',
		// home: HomePage(),
		initialRoute: '/',
		routes: {
		'/': (context) => HomePage()
		'/counter': (context) => SecondPage()
		}	  
	  )
  }
```

В такому підході ми можемо ще використати таку властивість `MaterialApp` як `initialRoute:` замість `home:`. Вона буде ініціалізувати початкову сторінку за переданим її шляхом.

Після такої реалізації потрібно використовувати метод `.pushNamed()` де аргументом ми передаємо рядком назву шляху який існує в нас в `Map`.

```dart
onPressed: () {
	Navigator.of(context).pushNamed('/counter');
},
```

>Така реалізація підходить для середніх та малих проектів.
---
## Generated Routing 
- Має схожу структуру як іменований маршрут єдина відмінність що потрібна окрема функція в якій потрібно буде все налаштовувати 
  Приклад:
  ```dart
  @override
  Route onGenerationRoute(RouteSettings settings) {
	  switch (settings.name){
		  case '/':
			  return MaterrialPageRoute(
				  builder: (_) => BlocProvider.value(
			value: _counterBloc,
			child: HomePage(),
			),
			break
		);
		case '/counter':
			  return MaterrialPageRoute(
				  builder: (_) => BlocProvider.value(
			value: _counterBloc,
			child: SecondPage(),
			),
			break
		);
	  }
	  return null
  }
```

>Така реалізація підходить для великих проектів.
---
# Отримання даних з іншої сторінки

- Коли нам потрібно отримати данні з однієї сторінки назад на іншу сторінку, можна передати значення в `pop` другим аргументом.
```dart
//Screen 2
onPressed: () {
 Navigator.pop(context, name);
},
```

- Також потрібно прийняти ці значення в тому `Navigator` який перенаправляє на іншу сторінку.
```dart
//Screen 1
 onPressed: () async {
  dynamic userName = await Navigator.push(context,
  MaterialPageRoute(builder: (context) {
  return const Screen2();
 }));
},
```

>Після цього можемо використовувати данні з `Screen 2` в `Screen 1` 