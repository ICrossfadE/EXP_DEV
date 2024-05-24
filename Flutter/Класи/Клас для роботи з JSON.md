```
class Todo {
  final String title;
  final String subtitle;
  bool isDone;

//constructor
  Todo({this.title = '', this.subtitle = '', this.isDone = false});

  Todo copyWith({String? title, String? subtitle, bool? isDone}) {
    return Todo(
        title: title ?? this.title,
        subtitle: subtitle ?? this.subtitle,
        isDone: isDone ?? this.isDone);
  }

  factory Todo.fromJson(Map<String, dynamic> json) {
    return Todo
        title: json['title'] as String? ?? '',
        subtitle: json['subtitle'] as String? ?? '',
        isDone: json['isDone'] as bool? ?? false);
  }

  Map<String, dynamic> toJson() {
    return {'title': title, 'subtitle': subtitle, 'isDone': isDone};
  }

  @override
  String toString() {
    return '''Todo: {
      'title': $title\n
      'subtitle': $subtitle\n
      'isDone': $isDone\n
    }''';
  }
}
```
---
## constructor
```
Todo({this.title = '', this.subtitle = '', this.isDone = false});
```
> Він використовує ініціалізацію через параметри конструктора в фігурних дужках (`{}`). Це називається *"Іменовані параметри"*.

Для кожного параметра (`title`, `subtitle`, `isDone`) задано значення за замовчуванням: порожні рядки. Для `isDone` бульове.

---

## copyWith
```
Todo copyWith({String? title, String? subtitle, bool? isDone}) {
    return Todo(
        title: title ?? this.title,
        subtitle: subtitle ?? this.subtitle,
        isDone: isDone ?? this.isDone);
  }
```
> Це метод, який дозволяє створити копію поточного об'єкта `Todo`, при цьому можна змінити деякі поля, залишивши інші незмінними.

 Параметри (тип `String?` і `bool?` означає, що параметри можуть бути або певного типу, або `null`).

Оператор ?? для кожного параметра. Якщо передане значення не `null`, використовується воно, інакше береться поточне значення відповідного поля об'єкта.

---

## Висновок

Клас `Todo` представляє завдання з трьома полями: `title`, `subtitle` та `isDone`. Конструктор дозволяє створювати об'єкти з заданими або значеннями за замовчуванням. Метод `copyWith` дозволяє створювати нові об'єкти `Todo` з оновленими значеннями деяких полів, зберігаючи інші поля без змін.

---