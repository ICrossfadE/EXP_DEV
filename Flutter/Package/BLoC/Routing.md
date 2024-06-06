>Є три типи маршрутизатора `Anonymos Routing` `Named Routing` `Genrated Routing`.

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

>Така реалізація може підійти для малих проектів
## Named Routing
- Це маршрут з додаванням назви. Всі маршрути які будуть вводитись мають бути іменованими та мають бути встановлені в параметрі `routes` у віджеті додатку
  приклад: 
  ```dart
  @override
  Widget build(BuildContext context) {
	  return MaterialApp(
		title: 'Flutter Demo',
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


>Така реалізація підходить для середніх та малих проектів.

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