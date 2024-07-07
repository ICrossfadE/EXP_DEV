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

## 2 -  Вставляєм скопійований SHA1 в налаштування проекту

- Заходимо в налаштування проекту та скролимо до самого низу там буде меню 
  `SHA certificate fingerprints`.
- Вставляємо згенерований SHA1.
- `cd ..` В терміналі щоб повернутися в корінь проекту.

## 3 - Завантажуємо google-services.json 

- В налаштуваннях проекту завантажуємо `google-services.json`
- Вставляємо/Замінюємо файл за адресом `android > app`.
- Обов'язково в `google-services.json` поле `package_name` має містити реальну назву проекту як в налаштуваннях Firebase.
```dart
"package_name": "com.coinkeep.app",
```

## 4 - Створюємо функціонал авторизації

- На власних платформах для запуску процесу автентифікації потрібна стороння бібліотека.
- Встановіть офіційний [`google_sign_in`](https://pub.dev/packages/google_sign_in)плагін.
- Після встановлення запустіть процес входу та створіть нові облікові дані:\

Необхідні пакети pubspec.yaml.
```dart
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

## 5 - Реалізація коду в Bloc та Cumbit 
...