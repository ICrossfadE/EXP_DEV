Міксин схожий до `extends` але його відмінність це те що він дозволяє розширюватись від декількох `mixin` в той момент як `extends` тільки від одного `class`.

Розширюватись від міксинів дозволяє ключове слова `with`.

```dart
void main() {
  Duck duck = Duck();
  duck.move();
  duck.swim();
  duck.fly();
}

class Animal {
  void move() {
    print('animal walk');
  }
}

mixin CanSwim {
  void swim() {
    print('animal swiming');
  }
}

mixin CanFly {
  void fly() {
    print('animal Flying');
  }
}

class Duck extends Animal with CanSwim, CanFly {}
```