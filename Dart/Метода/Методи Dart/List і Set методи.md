[DOC](https://dart.dev/libraries/dart-core#lists)

Список
```dart
var fruits = ['apples', 'oranges'];
```

add
```dart 
fruits.add('kiwis');
```
- Додає елемент в кінець списку.
---
addAll
```dart
fruits.addAll(['grapes', 'bananas']);
```
- Додає декілька елементів в кінець списку.
---
remove
```dart
fruits.remove('apples');
```
- Видалить переданий елемент якщо він присутній в масиві.
---
removeLast
```dart
fruits.removeLast();
```
- Видалить останній елемент в масиві.
---
shuffle
```dart
fruits.shuffle();
```
- Перемішає елементи в масиві.
---
removeAt()
```dart
fruits.removeAt(1);
```
- Видаляє по переданому індексу.
---
removeWhere()
```dart
fruits.removeWhere((element) => element == 'apples')
```
- Видаляє з масиву по переданій йому умові
---
first/.last
```dart
fruits.first
fruits.last
```
- Повертає перший або останній елемент
---
