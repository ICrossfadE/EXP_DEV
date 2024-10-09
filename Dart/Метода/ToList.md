`toList` використовується для перетворення об'єкта, що підтримує ітерації (наприклад, `Iterable`, таких як списки або результати запитів із `where`, `map` тощо), у список (`List`).

- `toList` створює новий список із елементів ітерації.
- Це корисно, коли потрібно конвертувати, наприклад, результат відбору (`where`) або перетворення (`map`) у фактичний список, оскільки зазвичай ці методи повертають об'єкти типу `Iterable`.

```dart
List<int> numbers = [1, 2, 3, 4, 5];

// Використовуємо where для фільтрації парних чисел
Iterable<int> evenNumbers = numbers.where((number) => number.isEven);

// Перетворюємо Iterable у List
List<int> evenNumbersList = evenNumbers.toList();

print(evenNumbersList); // Виведе: [2, 4]

```

### Як це працює:

1. `where((number) => number.isEven)` повертає об'єкт `Iterable`, який містить лише ті елементи, що відповідають умовам фільтрації (у цьому випадку — парні числа).
2. `toList()` перетворює результат цього фільтра на фактичний список.

### Особливості:

- **Створює новий список**: `toList` завжди повертає новий об'єкт `List`, навіть якщо він викликається на вже існуючому списку.
- **Копіює елементи**: Якщо ви викликаєте `toList` на існуючому списку або іншій колекції, новий список буде містити копії цих елементів.

### Приклад з перетворенням за допомогою `map`:
```dart
List<int> numbers = [1, 2, 3];

// Перетворюємо кожне число в рядок
Iterable<String> stringNumbers = numbers.map((number) => 'Number $number');

// Перетворюємо Iterable на List
List<String> stringNumbersList = stringNumbers.toList();

print(stringNumbersList); // Виведе: ['Number 1', 'Number 2', 'Number 3']

```

### Приклад з `Set`:
```dart
Set<String> fruits = {'apple', 'banana', 'orange'};

// Перетворюємо Set у List
List<String> fruitsList = fruits.toList();

print(fruitsList); // Виведе: ['apple', 'banana', 'orange']

```