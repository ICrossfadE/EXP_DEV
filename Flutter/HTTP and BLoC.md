Потрібні пакети
	[Http](https://pub.dev/packages/http)
	[Dio](https://pub.dev/packages/dio)

---
Для початку роботи нам потрібно буде потрібне API та створити JSON модель.
- API Використовуємо яке нам потрібно
- Далі створюємо JSON модель 
	1. Створити можна тут [JSON to Dart](https://javiercbk.github.io/json_to_dart/)
	2. Переносимо готову модель у наш фаїл.
    3. Якщо потрібні додаткові методи то створюємо їх.
- Створюємо папку з двома файлами `api_privider` і `api_repository` 
- `api_privider` 
  ```dart
import 'package:CoinKeep/data/coin_model.dart';
import 'package:dio/dio.dart';

class ApiProvider {
  final Dio _dio = Dio();
  final String _url =
      'https://pro-api.coinmarketcap.com/v1/cryptocurrency/listings/latest';
  final String _apiKey = '';

  Future<CoinModel> fetchCoins() async {
    try {
      _dio.options.headers['X-CMC_PRO_API_KEY'] = _apiKey;
      Response response = await _dio.get(_url);
      return CoinModel.fromJson(response.data);
    } catch (error, stacktrace) {
      print("Exception occured: $error stackTrace: $stacktrace");
      return CoinModel.withError("Data not found / Connection issue");
    }
  }
}
```
- `_dio`: Екземпляр класу `Dio`, який використовується для виконання HTTP-запитів.
- `_url`: URL для отримання останніх даних.
- `_apiKey`: API ключ для автентифікації.
- `try` 
  1. Додає API ключ до headers запиту.
  2. Виконує GET запит до зазначеного URL за допомогою `Dio`.
  3. Перетворює отримані дані з JSON у об'єкт `CoinModel`.
- `catch`
  1. У разі виникнення помилки (наприклад, проблеми з підключенням), ловить виключення та друкує повідомлення про помилку разом зі стеком викликів.
  2. Повертає об'єкт `CoinModel` з повідомленням про помилку.
---
- `api_repository`  
```dart
import 'package:CoinKeep/data/coin_model.dart';
import 'package:CoinKeep/resource/api_provider.dart';

class ApiRepository {
  final ApiProvider _apiProvider = ApiProvider();

  Future<CoinModel> fetchCoins() {
    return _apiProvider.fetchCoins();
  }
}

class NetworkError extends Error {}
```
>Цей код визначає клас `ApiRepository`, який служить репозиторієм для отримання даних з API за допомогою `ApiProvider`. Він також визначає клас `NetworkError` для обробки мережевих помилок.

`ApiRepository` є посередником між додатком і `ApiProvider`. Він інкапсулює логіку отримання даних з API і надає її для використання іншими частинами додатка.
- `_apiProvider`: Екземпляр `ApiProvider`, який виконує фактичні запити до API.
- `Future<CoinModel> fetchCoins()`: Метод, який викликає метод `fetchCoins` з `ApiProvider` для отримання даних про криптовалюти.
- `NetworkError` є користувацьким класом помилок, який успадковується від `Error`. Він може бути використаний для індикації специфічних помилок, пов'язаних з мережею.

### Переваги використання репозиторію

1. **Інкапсуляція логіки доступу до даних:** Репозиторій інкапсулює всю логіку доступу до даних, роблячи ваш код чистішим і більш організованим. Наприклад, ви можете змінювати джерело даних (з API на локальну базу даних) без необхідності змінювати код у місцях, де використовується репозиторій.

2. **Зменшення повторення коду:** Репозиторій може містити спільну логіку доступу до даних, яку можна використовувати в різних частинах вашого додатка, зменшуючи дублювання коду.

4. **Тестування:** Використання репозиторію спрощує тестування коду. Ви можете створювати мок-версії репозиторію для юніт-тестів, що дозволяє ізолювати та тестувати логіку вашого додатка без залежності від реальних даних або мережевих запитів.

4. **Зміна джерела даних:** Репозиторій полегшує зміну джерела даних. Якщо вам потрібно перейти з одного API на інше або додати кешування, ви можете зробити це в одному місці — у репозиторії.
---
## Створюємо папку bloc
- Створюємо паку `bloc` та три файла в ній `_bloc`,`_event`,`_state`
### Event
```dart
part of 'transactions_bloc.dart';

abstract class TransactionsEvent extends Equatable {
  const TransactionsEvent();

  @override
  List<Object> get props => [];
}

class GetCoins extends TransactionsEvent {}

```
- Цей код визначає події для блока `transactions_bloc.dart` у Flutter/Dart, використовуючи бібліотеку `equatable`, яка полегшує порівняння об'єктів.

1. **`abstract class TransactionsEvent extends Equatable`**
    - Визначає абстрактний клас `TransactionsEvent`, який наслідується від `Equatable`. `Equatable` використовується для спрощення порівняння об'єктів у Dart, щоб легко визначати, чи є два екземпляри одного класу рівними.
    - Абстрактний клас означає, що не можна створювати екземпляри цього класу напряму, але можна створювати екземпляри класів, які наслідуються від нього.
2. **`const TransactionsEvent();`**
    - Конструктор для класу `TransactionsEvent`. Він є `const`, що означає, що створені об'єкти цього класу будуть незмінними (іммутабельними).
3. **`@override List<Object> get props => [];`**
    - Перевизначає метод `props` з `Equatable`, який повертає список властивостей, що використовуються для порівняння об'єктів. У даному випадку він повертає порожній список, що означає, що всі екземпляри `TransactionsEvent` будуть рівні між собою, оскільки вони не мають власних властивостей для порівняння.
3. **`class GetCoins extends TransactionsEvent {}`**
    - Визначає клас `GetCoins`, який наслідується від `TransactionsEvent`. Це конкретна подія, яка може бути використана для отримання даних про криптовалюти.

### State
```dart
part of 'transactions_bloc.dart';

abstract class TransactionsState extends Equatable {
  const TransactionsState();

  @override
  List<Object> get props => [];
}

class TransactionsInitial extends TransactionsState {}

class TransactionsLoading extends TransactionsState {}

class TransactionsLoaded extends TransactionsState {
  final CoinModel coinModel;
  const TransactionsLoaded(this.coinModel);
}

class TransactionsError extends TransactionsState {
  final String? error;
  const TransactionsError(this.error);
}
```
>Цей код визначає різні стани для BLoC у Dart/Flutter.

1. **`TransactionsInitial`**
    - Представляє початковий стан блока. Це стан до будь-якої взаємодії з блоком.
2. **`TransactionsLoading`**
	- Представляє стан завантаження. Використовується, коли дані завантажуються з API або іншого джерела.
3. **`TransactionsLoaded`**
	- Представляє стан успішного завантаження даних.
4. **`TransactionsError`**
    - Представляє стан помилки.

>Класи станів (`TransactionsState`, `TransactionsInitial`, `TransactionsLoading`, `TransactionsLoaded`, `TransactionsError`) представляють різні стани, в яких може перебувати ваш блок. Вони забезпечують чисту і організовану структуру для управління станом у додатку, що використовує шаблон BLoC у Flutter.

---
### Bloc
```dart
import 'package:bloc/bloc.dart';
import 'package:equatable/equatable.dart';
import 'package:CoinKeep/data/coin_model.dart';
import 'package:CoinKeep/resource/api_repository.dart';

part 'transactions_event.dart';
part 'transactions_state.dart';

class TransactionsBloc extends Bloc<TransactionsEvent, TransactionsState> {
  TransactionsBloc() : super(TransactionsInitial()) {
    on<GetCoins>(_onGetCoins);
  }

  final ApiRepository _apiRepository = ApiRepository();

  void _onGetCoins(GetCoins event, Emitter<TransactionsState> emit) async {
    try {
      emit(TransactionsLoading());
      final coinList = await _apiRepository.fetchCoins();
      emit(TransactionsLoaded(coinList));
      if (coinList.error != null) {
        emit(TransactionsError(coinList.error));
      }
    } on NetworkError {
      emit(TransactionsError('Failed to fetch data. is your device online?'));
    }
  }
}
```
>  Цей код реалізує BLoC (Business Logic Component) для управління станом транзакцій в додатку, який використовує бібліотеки `bloc` і `equatable` у Flutter/Dart.

1. **Клас `TransactionsBloc`:**
    - Наслідується від `Bloc<TransactionsEvent, TransactionsState>`, що означає, що він буде обробляти події типу `TransactionsEvent` і змінювати стан типу `TransactionsState`
    - Початковий стан встановлюється через `super(TransactionsInitial())`.
2. **Конструктор `TransactionsBloc`:**
    - Викликає метод `on<GetCoins>` для обробки події `GetCoins`, використовуючи метод `_onGetCoins`.
3. **Змінна `_apiRepository`:**
    - Ініціалізує екземпляр `ApiRepository`, який використовується для отримання даних.
4. **Метод `_onGetCoins`:**
    - Обробляє подію `GetCoins`.
    - Встановлює стан `TransactionsLoading`.
    - Викликає метод `fetchCoins` з `_apiRepository`.
    - Якщо дані успішно отримані і немає помилки, встановлюється стан `TransactionsLoaded`.
    - Якщо є помилка, встановлюється стан `TransactionsError` з повідомленням про помилку.
    - Якщо виникає `NetworkError`, встановлюється стан `TransactionsError` з повідомленням про мережеву помилку.