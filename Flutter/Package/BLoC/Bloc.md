Мета цього пакета — полегшити реалізацію `Bloc` шаблону проектування (компонента бізнес-логіки).

Цей шаблон проектування допомагає відокремити _презентацію_ від _бізнес-логіки_ . Дотримання шаблону `Bloc` полегшує тестування та повторне використання. Цей пакет абстрагує реактивні аспекти шаблону, дозволяючи розробникам зосередитися на написанні бізнес-логіки.
![[Pasted image 20240605135348.png]]
## Business Layer
Тут розташовано більшість блоків. Основний обов'язок цього рівня реагувати на дії користувача які приходять з `Presentation Layer`. Якщо коротко то це посередник між `UI`та `Data`