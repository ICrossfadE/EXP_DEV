### runApp();

Запускає в собі ввесь додаток. Запускається в основній функції `main`.
```dart
void main() {
	runApp(const MyApp())
}
```
---
```dart
void main() {
	WidgetsFlutterBinding.ensureInitialized()
	runApp(const MyApp())
}
```

**`WidgetsFlutterBinding.ensureInitialized()`**:

- Ця функція забезпечує правильну ініціалізацію Flutter додатку перед виконанням асинхронного коду. Це обов'язково, якщо ви використовуєте будь-який асинхронний код до виклику `runApp`.
---
```dart
void main() {
	await Firebase.initializeApp()
	runApp(const MyApp())
}
```

**`await Firebase.initializeApp()`**:

- Ініціалізує Firebase в додатку. Це потрібно для того, щоб використовувати сервіси Firebase, такі як аутентифікація, база даних тощо.