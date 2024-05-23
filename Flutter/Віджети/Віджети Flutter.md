
> [!NOTE] Widget
> Можна сказати що це заготовлені об'єкти які пізніше демонструються користувачеві


> [!NOTE] Sacffold()
> Віджет який дозволяє виводити в собі багато віджетів.

```
class myAppToDo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(primaryColor: Color.fromARGB(255, 45, 53, 168)),
      home: Scaffold(),
    );
  }
}
```