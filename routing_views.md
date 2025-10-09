# Руководство по созданию новых страниц в проекте Flutter

**[🏠 Вернуться на главную](README.md)**

## Содержание

1. [Что такое страницы в Flutter?](#что-такое-страницы-в-flutter)
2. [Создание новой страницы](#создание-новой-страницы)
3. [Навигация между страницами](#навигация-между-страницами)
4. [Передача данных между страницами](#передача-данных-между-страницами)
5. [Пример виджета с переходом](#пример-виджета-с-переходом)

---

## Что такое страницы в Flutter?

В Flutter "страница" — это отдельный экран приложения, который обычно реализуется как виджет (`StatelessWidget` или `StatefulWidget`). Приложение может состоять из нескольких страниц, между которыми пользователь переключается, например, с главной
страницы на страницу настроек или профиля.

Flutter использует `Navigator` для управления стеком страниц: вы можете "толкать" (push) новую страницу на стек или "выталкивать" (pop) текущую, чтобы вернуться назад. Это похоже на навигацию в веб-браузере.

Для работы с несколькими страницами в приложении обычно используется виджет `MaterialApp`, который поддерживает маршруты (routes) — именованные пути к страницам.

---

## Создание новой страницы

Чтобы создать новую страницу, создайте отдельный класс виджета. Разместите его в новом файле Dart (например, `second_page.dart`) в папке `lib` вашего проекта.

### Пример: Простая вторая страница (StatelessWidget)

```dart
import 'package:flutter/material.dart';

class SecondPage extends StatelessWidget {
  const SecondPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Вторая страница'),
      ),
      body: const Center(
        child: Text('Это вторая страница приложения!'),
      ),
    );
  }
}
```

- **Scaffold**: Основная структура страницы с AppBar (верхняя панель) и body (основное содержимое).
- Разместите этот код в новом файле и импортируйте его в основной файл приложения, если нужно.

Если страница должна изменяться (например, с кнопками или формами), используйте StatefulWidget, как описано в [Руководстве по созданию виджетов](flutter_widgets_guide_create.md).

---

## Навигация между страницами

### Базовая навигация с Navigator

Для создания страниц в приложении используйте именованные маршруты в MaterialApp.

> Обратите внимание на то, что страницу необходимо для начала импортировать ваш виджет в `main.dart`

#### Пример в main.dart:

```dart
import 'package:flutter/material.dart';
import 'second_page.dart'; // Импорт вашей страницы

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      
      // Отвечает за регистрацию ваших страниц в приложении
      routes: {
        '/': (context) => const HomePage(),
        '/second': (context) => const SecondPage(),
      },
    );
  }
}
```

- Переход по имени: `Navigator.pushNamed(context, '/second');`

Это упрощает управление множеством страниц.

---

## Передача данных между страницами

Часто нужно передать данные на новую страницу (например, имя пользователя).

Для начала необходимо добавть параметр в конструктор страницы.

#### В SecondPage:

```dart
class SecondPage extends StatelessWidget {
  String? message;

  const SecondPage({super.key, required this.message});

  // Данная функция используется для извелечения аргументов при открытии маршрута
  @override
  void didChangeDependencies() {
    final args = ModalRoute.of(context)?.settings.arguments;

    setState(() {
        name = args as String;
    });
    super.didChangeDependencies();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(child: Text(message)),
    );
  }
}
```

- Переход по имени с использование аргументов `Navigator.pushNamed(context,'/detail', arguments: 'Hello');`

---

## Пример виджета с переходом

```dart
class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushNamed(context, '/second');
          },
          child: const Text('Перейти на вторую страницу'),
        ),
      ),
    );
  }
}
```

**[🏠 Вернуться на главную](README.md)**
