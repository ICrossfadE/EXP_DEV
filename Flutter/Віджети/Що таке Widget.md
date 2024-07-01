Віджети в Flutter це класи які мають власні атрибути та методи. Ці класи можуть бути створені як об'єкти визначивши їх необхідні параметри та методи в конструкторі.

## Приклад
```dart
class Card extends StatelessWidget {
  final Color selectColor;
  final Widget? cardChild;

  const Card({super.key, required this.selectColor, this.cardChild});

  @override
  Widget build(BuildContext context) {
    return Container(
      child: cardChild,
      margin: const EdgeInsets.all(15.0),
      decoration: BoxDecoration(
        color: selectColor,
        borderRadius: BorderRadius.circular(10.0),
      ),
    );
  }
}
```
- Створення класу Card, який успадковується від `StatelessWidget`. Це означає, що віджет є незмінним (stateless) і не має внутрішнього стану, який може змінюватися з часом.
- В класі є дві властивості
	- `selectColor`: колір карти (обов'язковий параметр) `required`
	- `cardChild`: дочірній віджет, який буде розміщено всередині карти (необов'язковий параметр)
- Конструктор приймає named параметри:
	- `key`: ключ віджета (передається суперкласу)
	- `selectColor`: обов'язковий параметр для кольору `required`
	- `cardChild`: необов'язковий параметр для дочірнього віджета
- Метод `build`:
	- Цей метод відповідає за створення і повернення віджета, який буде відображатися на екрані.