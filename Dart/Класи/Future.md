>`Future` в Dart — це клас, який представляє відкладений результат асинхронної операції. Він використовується для роботи з асинхронними операціями, такими як виконання HTTP-запитів, читання файлів, затримки та інші операції, які можуть виконуватися тривалий час.

1. **Створення `Future`:**
    - `Future` може бути створений безпосередньо або за допомогою методів, які повертають `Future`.
    - Наприклад, `Future.delayed(Duration(seconds: 2))` створює `Future`, який завершується через 2 секунди.
2. **Асинхронні функції:**
    - Функції, які використовують ключове слово `async`, автоматично повертають `Future`.
    - Ключове слово `await` використовується всередині `async` функцій для очікування завершення `Future` і отримання його результату.
## Приклад
```dart
Future<void> main() async {
  try {
    print('Fetching data...');
    String data = await fetchData();
    print('Data received: $data');
  } catch (error) {
    print('Error: $error');
  }
}

Future<String> fetchData() async {
  await Future.delayed(Duration(seconds: 2));
  throw Exception('Failed to fetch data');
}


```