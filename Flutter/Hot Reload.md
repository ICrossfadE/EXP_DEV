Коли ми оновлюємо додаток через `hot reload` тоді буде оновлюватись тільки те що всередині `Widget build` 
```dart
@override
  Widget build(BuildContext context) {}
```

Якщо в процесі розробки створені нові зміні за межами `Widget build` 

```dart
var count = 1;

@override
  Widget build(BuildContext context) {}
```

потрібно перевантажити додаток.

**Також `Hot Reload` зберігає зміни стейту**
