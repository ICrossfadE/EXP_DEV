>Це один з основних віджетів у Flutter, який забезпечує структуру та компонування основних елементів інтерфейсу додатка. Він надає стандартний макет для додатків Material Design і містить кілька корисних властивостей, які дозволяють легко створювати складні інтерфейси.

- `Scaffold` надає структуру, яка містить різні частини інтерфейсу, такі як панель додатка (app bar), висувну панель (drawer), плаваючу кнопку дії (floating action button) і основний контент (body). Ось основні компоненти, які можна використовувати зі `Scaffold`:

- **appBar**:
	- Віджет `AppBar` для відображення заголовка додатка, кнопок дій і інших елементів управління.

```
appBar: AppBar(
  title: Text('My App'),
),
```
- **body**:
	- Основний контент додатка. Це може бути будь-який віджет, який відображає основну інформацію.

```
body: Center(
  child: Text('Hello, world!'),
),
```
- **floatingActionButton**:
	- Плаваюча кнопка дії, зазвичай використовується для основних дій на екрані.

```
floatingActionButton: FloatingActionButton(
  onPressed: () {},
  child: Icon(Icons.add),
),
```
- **drawer**:
	- Висувна панель з лівого боку екрана, зазвичай використовується для навігації.

```
drawer: Drawer(
  child: ListView(
    children: <Widget>[
      DrawerHeader(
        child: Text('Header'),
      ),
      ListTile(
        title: Text('Item 1'),
        onTap: () {},
      ),
    ],
  ),
),
```
- **bottomNavigationBar**:
	- Нижня панель навігації, яка дозволяє переключатися між різними розділами додатка.

```
bottomNavigationBar: BottomNavigationBar(
  items: [
    BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Home'),
    BottomNavigationBarItem(icon: Icon(Icons.search), label: 'Search'),
  ],
),
```
- **bottomSheet**:
	- Стане нижньої панелі, яка може бути постійною або модальною.

```
bottomSheet: BottomSheet(
  onClosing: () {},
  builder: (context) => Container(
    height: 200,
    color: Colors.white,
    child: Center(child: Text('Bottom Sheet')),
  ),
),
```
- **backgroundColor**:
	- Колір фону для `Scaffold`.

```
backgroundColor: Colors.blueGrey,
```