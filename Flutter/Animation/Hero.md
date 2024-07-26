Віджет який дочірнім елементом приймає в себе віджет який буде анімуватися.
```dart
//Page 1
Hero(
  tag: 'logo',
  child: SizedBox(
	height: 60,
	child: Image.asset(
	  'images/logo.png',
	),
  ),
)
```
Також має бути аналогічний елемент на іншій сторінці. Пара віджетів має мати спільний `tag`
```dart
//Page 2
Hero(
  tag: 'logo',
  child: SizedBox(
	height: 200,
	child: Image.asset(
	  'images/logo.png',
	),
  ),
)
```

Після реалізації між сторінками створиться перехідна сторінка яка буде плавно анімувати зміни розміру картинки.
