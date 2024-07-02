Class це шаблонний підхід для створення нових даних.

Class має містити значення різного типу
- Назва класу повинна бути з великої букви.
```dart
class User {
	String name;
	int age;
	bool isHappy;
	List<String> hobnies;
}
```
Також клас може містити методи
```dart
class User {
	String name;
	int age;
	bool isHappy;
	List<String> hobnies;
	
	void userName() {
		print(this.name)
	}
}
```
- **Якщо метод повертатиме `bool` то потрібно поставити `bool?`.**
- **`bool?` означає, що значення може бути `true`, `false`, або `null`.**

В Dart при створені нового екземпляру непотрібно оголошувати `new`.
```dart
User nik = User()
```

Після об'яви нового об'єкту `nik` на основі класу `User` можемо через `..` одразу зробити присвоєння значень ключам об'єкту. 
- Таким чином можна створювати послідовність виконань після створення об'єкту.
```dart
var nik = User()
    ..name = 'NIK'
    ..age = 20
    ..isHappy = true
    ..hobbies = ['coding', 'hiking', 'photos'];
```

**тепер в нас є готовий об'єкт на основі екземпляру класу `User`**

---
## constructor

Може приймати обов'язкові та не обов'язкові параметри в якості аргументів.
Необов'язковий параметри огортають в `[]`.
```dart
class User {
  String name = '';
  int age = 0;
  bool isHappy = false;
  List<String> hobbies = [];

  //constructor
  User(name, [age, isHappy, hobbies]);
}
```

Без обов'язкового параметру `name` буде помилка.

```dart
void main() {
  var nik = User('NIK', 20, true, ['coding', 'hiking', 'photos']);
  
  print(nik.age);
}

class User {
  String name = '';
  int age = 0;
  bool isHappy = false;
  List<String> hobbies = [];

  User(this.name, this.age, this.isHappy, this.hobbies);
}
```

За наявності конструктора потрібно для не обов'язкових значень конструктора які обгорнуті в `[]` присвоювати стандартні значення без них буде помилка

```dart
void main() {
  var nik = User('NIK');
  
  print(nik.age);
}

class User {
  String name;
  int age;
  bool isHappy;
  List<String> hobbies;

  User(this.name, [this.age = 0, this.isHappy = false, this.hobbies = const ['skate']]);
}
```

---
