Архітектура BLoC розрахована на то що будуть створюватись декілька блоків.

Під час використання `MultiBlocProvider` ним потрібно огорнути звичайний `BlocProvider`.
   ```dart
   Widget build(BuildContext context) {
    return MultiBlocProvider(
      providers: [
        BlocProvider<CounterBloc>(
          create: (context) => bloc,
        ),
        BlocProvider<TransactionsBloc>(
          create: (context) => TransactionsBloc(),
        ),
	   ]
       };
```
Після цього створюєм папку `block` в `lib` та 3 файла в ньому 
- `Назва_bloc.dart`
- `Назва_event.dart`
- `Назва_state.dart`
>Якщо в VSCode є розширення то можна через нього зробити це акуратніше та швидше.