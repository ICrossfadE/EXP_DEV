Покрокова інструкція по підключенню авторизації `Google provider`

[Firebase Doc](https://launchpad.gmnetwork.ai/mission)
[Generate SHA1 finger print](https://developers.google.com/android/guides/client-auth?authuser=0)

## 1 - Згенерувати SHA-1 за Gradle.

Обов'язковий пункт. Без нього авторизація не буде коректо працювати.
- Заходимо в папку проекту `android > gredlew` та відкриваємо його в терміналі.
- `./gradlew signingReport` - виконуємо цю команду в терміналі.
```dart
Variant: debugConfig: 
debugStore: ~/.android/debug.keystoreAlias: 
AndroidDebugKeyMD5: A5:88:41:04:8D:06:71:6D:FE:33:76:87:AC:AD:19:23
SHA1:A7:89:E5:05:C8:17:A1:22:EA:90:6E:A6:EA:A3:D4:8B:3A:30:AB:18
SHA256:05:A2:2C:35:EE:F2:51:23:72:4D:72:67:A5:6C:8C:58:22:2A:00:D6:DB:F6:45:D5:C1:82:D2:80:A4:69:A8
FEValid until: 
Wednesday, August 10, 2044
```
*Отримуємо подібний результат, копіюємо SHA1*

---
## 2 -  Вставляєм скопійований SHA1 в налаштування проекту

- Заходимо в налаштування проекту та скролимо до самого низу там буде меню 
  `SHA certificate fingerprints`.
- Вставляємо згенерований SHA1.
- `cd ..` В терміналі щоб повернутися в корінь проекту.
---
## 3 - Завантажуємо google-services.json 

- В налаштуваннях проекту завантажуємо `google-services.json`
- Вставляємо/Замінюємо файл за адресом `android > app`.
- Обов'язково в `google-services.json` поле `package_name` має містити реальну назву проекту як в налаштуваннях Firebase.
```dart
"package_name": "com.coinkeep.app",
```

---
## 4 - Модель User якого отримуватимо з Firebase.

```dart
import 'package:equatable/equatable.dart';

class MyUserEntity extends Equatable {
  final String userId;
  final String? email;
  final String? name;
  final String? photoUrl;

  const MyUserEntity({
    required this.userId,
    this.email,
    this.name,
    this.photoUrl,
  });

  // Перетворюємо в JSON для відправки у Firestore
  Map<String, Object?> toDocument() {
    return {
      'userId': userId,
      'email': email,
      'name': name,
      'photoUrl': photoUrl,
    };
  }

  // Перетворюємо в обєкт з Firestore
  static MyUserEntity fromDocument(Map<String, dynamic> document) {
    return MyUserEntity(
      userId: document['userId'],
      email: document['email'],
      name: document['name'],
      photoUrl: document['photoUrl'],
    );
  }

  @override
  List<Object?> get props => [userId, email, name, photoUrl];
}
```

---
## 5 - Створюємо функціонал авторизації

- На власних платформах для запуску процесу автентифікації потрібна стороння бібліотека.
- Встановіть офіційний [`google_sign_in`](https://pub.dev/packages/google_sign_in)плагін.
- Після встановлення запустіть процес входу та створіть нові облікові дані:\

Необхідні пакети pubspec.yaml.
```yaml
  firebase_core: ^3.1.0
  firebase_auth: ^5.1.0
  google_sign_in: ^6.2.1
```

- Створюємо папку та в ній файл `FirebaseRepo.dart`.
```dart
// Реалізація функціоналу з UserRepository
class FirebaseUserRepo implements AuthReository {
  final FirebaseAuth _firebaseAuth;
  final usersCollection = FirebaseFirestore.instance.collection('users');

  // constructor
  FirebaseUserRepo({
    FirebaseAuth? firebaseAuth,
    //Передаємо інший екземпляр або використовує стандартний якщо не переданий.
  }) : _firebaseAuth = firebaseAuth ?? FirebaseAuth.instance;

  @override
  Future<UserCredential> signInWithGoogle() async {
    // Запустити потік автентифікації

    final GoogleSignInAccount? googleUser = await GoogleSignIn().signIn();
    print('Google user: ${googleUser?.email}');

    if (googleUser == null) {
      throw Exception('Google Sign In was canceled');
    }

    // Отримайте дані автентифікації із запиту
    final GoogleSignInAuthentication? googleAuth =
        await googleUser?.authentication;
    print('Got Google Auth');

    // Створіть нові облікові дані
    final credential = GoogleAuthProvider.credential(
      accessToken: googleAuth?.accessToken,
      idToken: googleAuth?.idToken,
    );
    print('Created credential');

    UserCredential userCredential =
        await _firebaseAuth.signInWithCredential(credential);

    User? user = userCredential.user;
    if (user != null) {
      // Ім'я
      String? name = user.displayName;
      // Email
      String? email = user.email;
      // URL фото профілю
      String? photoUrl = user.photoURL;
      log('User signed in: $name, $email, $photoUrl');
    }
    // Після входу поверніть UserCredential
    return userCredential;
  }

  @override
  Stream<User?> get user {
    return _firebaseAuth.authStateChanges().map((firebaseUser) {
      log('authStateChanges: $firebaseUser');
      return firebaseUser;
    });
  }

  @override
  Stream<bool> get isAuthenticated {
    return user.map((firebaseUser) {
      return firebaseUser != null;
    });
  }

  @override
  Future<void> logOut() async {
    await _firebaseAuth.signOut();
  }
}
```

- Також створіть клас імплементації.
```dart
// Репозиторій функціоналу по аутентифікації користувача
abstract class AuthReository {
  Stream<User?> get user; // Потік змін стану користувача
  Stream<bool> get isAuthenticated;

  Future<UserCredential> signInWithGoogle();
  Future<void> logOut(); // Вихід користувача
}
```
---
## 6 - Реалізація коду в Bloc та Cumbit 
- Підключаємо Bloc.
```yaml
 bloc: ^8.1.4
 equatable: ^2.0.5
```
- Створюємо папку auth-bloc
- В ній файли `auth_bloc.dart`, `auth_event.dart`, `auth_state.dart`.
---
`auth_state.dart`
```dart
part of 'auth_google_bloc.dart';

enum AuthStatus { authenticated, unauthenticated, unknown }

class AuthGoogleState extends Equatable {
  //AuthState._ є приватним конструктором.
  const AuthGoogleState._({
    this.status = AuthStatus.unknown,
    this.user,
  });

  final AuthStatus status;
  final User? user;

  // статус невідомий
  const AuthGoogleState.unknown() : this._();

  // Статус авторизованого користувача
  const AuthGoogleState.authenticated(User user)
      : this._(status: AuthStatus.authenticated, user: user);

  // Статут не авторизований
  const AuthGoogleState.unauthenticated()
      : this._(status: AuthStatus.unauthenticated);

  @override
  List<Object?> get props => [status, user];
}
```
---
`auth_event.dart`
```dart
part of 'auth_google_bloc.dart';

class AuthGoogleEvent extends Equatable {
  const AuthGoogleEvent();

  @override
  List<Object> get props => [];
}

final class AppLogoutRequested extends AuthGoogleEvent {
  const AppLogoutRequested();
}

final class AppUserChanged extends AuthGoogleEvent {
  const AppUserChanged(this.user);

  final User? user;
}
```
---
`auth_bloc.dart`
```dart
part 'auth_google_event.dart';
part 'auth_google_state.dart';

class AuthGoogleBloc extends Bloc<AuthGoogleEvent, AuthGoogleState> {
  final AuthReository _authRepository;
  late final StreamSubscription<User?> _userSubscription;

  AuthReository get authRepository => _authRepository;

  AuthGoogleBloc({required AuthReository authRepository})
      : _authRepository = authRepository,
        super(const AuthGoogleState.unknown()) {
    on<AppUserChanged>(_onUserChanged);
    on<AppLogoutRequested>(_onLogoutRequested);
    _userSubscription = _authRepository.user.listen(
      (user) {
        print(
            'User state changed: ${user != null ? 'Authenticated' : 'Unauthenticated'}');
        add(AppUserChanged(user));
      },
    );
  }

  void _onUserChanged(AppUserChanged event, Emitter<AuthGoogleState> emit) {
    if (event.user != null) {
      emit(AuthGoogleState.authenticated(event.user!));
    } else {
      emit(const AuthGoogleState.unauthenticated());
    }
  }

  void _onLogoutRequested(
      AppLogoutRequested event, Emitter<AuthGoogleState> emit) {
    unawaited(_authRepository.logOut());
  }

  @override
  Future<void> close() {
    _userSubscription.cancel();
    return super.close();
  }
}
```
---
- Створюємо Cumbut папку `login_google`
- Створюємо файли `auth_exceptions.dart`, `login_cubit.dart`, `login_state.dart`,
---
`auth_exceptions.dart`
```dart
class LogInWithGoogleFailure implements Exception {
  final String message;

  const LogInWithGoogleFailure([this.message = 'An unknown error occurred.']);

  factory LogInWithGoogleFailure.fromCode(String code) {
    switch (code) {
      case 'account-exists-with-different-credential':
        return const LogInWithGoogleFailure(
          'Account exists with different credentials.',
        );
      case 'invalid-credential':
        return const LogInWithGoogleFailure(
          'The credential received is malformed or has expired.',
        );
      case 'operation-not-allowed':
        return const LogInWithGoogleFailure(
          'Operation is not allowed.  Please contact support.',
        );
      case 'user-disabled':
        return const LogInWithGoogleFailure(
          'This user has been disabled. Please contact support for help.',
        );
      case 'user-not-found':
        return const LogInWithGoogleFailure(
          'Email is not found, please create an account.',
        );
      case 'wrong-password':
        return const LogInWithGoogleFailure(
          'Incorrect password, please try again.',
        );
      case 'invalid-verification-code':
        return const LogInWithGoogleFailure(
          'The credential verification code received is invalid.',
        );
      case 'invalid-verification-id':
        return const LogInWithGoogleFailure(
          'The credential verification ID received is invalid.',
        );
      default:
        return const LogInWithGoogleFailure();
    }
  }
}
```
---

`login_state.dart`
```dart
part of 'login_cubit.dart';

enum LoginStatus { initial, inProgress, success, failure }

final class LoginState extends Equatable {
  const LoginState({
    this.status = LoginStatus.initial,
    this.isValid = false,
    this.errorMessage,
  });

  final LoginStatus status;
  final bool isValid;
  final String? errorMessage;

  @override
  List<Object?> get props => [status, isValid, errorMessage];

  LoginState copyWith({
    LoginStatus? status,
    bool? isValid,
    String? errorMessage,
  }) {
    return LoginState(
      status: status ?? this.status,
      isValid: isValid ?? this.isValid,
      errorMessage: errorMessage ?? this.errorMessage,
    );
  }
}
```
---
`login_cubit.dart`
```dart
part 'login_state.dart';

class LoginCubit extends Cubit<LoginState> {
  LoginCubit(this._authReository) : super(const LoginState());

  final AuthReository _authReository;

  Future<void> logInWithGoogle() async {
    emit(state.copyWith(status: LoginStatus.inProgress));
    try {
      print('Starting Google Sign In');
      await _authReository.signInWithGoogle();
      print('Google Sign In Successful');
      emit(state.copyWith(status: LoginStatus.success));
    } on LogInWithGoogleFailure catch (e) {
      print('Google Sign In Failed: ${e.message}');
      emit(
        state.copyWith(
          errorMessage: e.message,
          status: LoginStatus.failure,
        ),
      );
    } catch (e) {
      print('Unexpected error during Google Sign In: $e');
      emit(state.copyWith(status: LoginStatus.failure));
    }
  }
}
```
---


## 7 - Ініціалізуємо Firebase в main();

```dart
void main() async {
  await Firebase.initializeApp();
  
  runApp(MyApp(FirebaseUserRepo()));
}
```
---
## 8 - Передаємо bloc в BlocProvider;

```dart
class MyApp extends StatelessWidget {
  //Отримуємо екземпляр authReository
  final AuthReository authReository;
  const MyApp(this.authReository, {super.key});

  

  @override
  // Головний Інтерфейс
  Widget build(BuildContext context) {
    return MultiRepositoryProvider(
      providers: [
        RepositoryProvider.value(value: authReository),
      ],
      child: MultiBlocProvider(
        providers: [
          BlocProvider<AuthGoogleBloc>(
            create: (context) => AuthGoogleBloc(authRepository: authReository),
          ),
        ],
        child: const AppView(),
      ),
    );
  }
}
```
- Оголошуємо `MultiRepositoryProvider` якщо потрібно в майбутньому передати декілька bloc.
---
## 9 - Реалізуємо BlocBilder;

Реалізуємо `BlocBuilder` для динамічної рендера певного місця в коді а не всього перерендуру. 
```dart
home: BlocBuilder<AuthGoogleBloc, AuthGoogleState>(
	builder: (context, state) {
	  if (state.status == AuthStatus.authenticated) {
		return BlocProvider(
		  create: (context) => AuthGoogleBloc(
			authRepository: context.read<AuthGoogleBloc>().authRepository,
		  ),
		  child: const DashboardPage(),
		);
	  } else {
		return BlocProvider(
		  create: (_) => LoginCubit(context.read<AuthReository>()),
		  child: const AuthPage(),
		);
	  }
	},
  ),
```
- Далі якщо ми хочемо звернутися до даних `User` на іншій сторінці, до прикладу `DashboardPage()`. Там нам теж потрібно огорнути потрібний елемент в `BlocBuilder`.

```dart
child: BlocBuilder<AuthGoogleBloc, AuthGoogleState>( 
	builder: (context, state) { 
		if (state is AuthGoogleState) { 
			final user = state.user;
			return Widget()
```
- Тепер `Widget` матиме доступ до `User` і зараз ми можемо отримати до прикладу фото користувача `user.photoURL`.

---

## 10 - Реалізуємо LogIn;

Сторінка `AuthPage()`
```dart
 ElevatedButton(
  onPressed: () {
	context.read<LoginCubit>().logInWithGoogle();
  },
  child: Text('Sign in with Google'),
)
```

---
## 11 - Реалізуємо LogOut;

```dart

 ElevatedButton(
  onPressed: () {
	context
		.read<AuthGoogleBloc>()
		.add(const AppLogoutRequested());
  },
  child: Text('Log Out'),
)
```
- В потрібному місці реалізуємо код на кнопці.
---
