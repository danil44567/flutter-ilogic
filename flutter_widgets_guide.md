**[🏠 Вернуться на главную](README.md)**

# 🛠️ Основные инструменты в Flutter

## Содержание

- [Поле ввода: TextField](#поле-ввода-textfield)
- [Кнопка: ElevatedButton](#кнопка-elevatedbutton)
- [Управление состоянием: setState](#setstate)

---

## Поле ввода: TextField

Для чтения текста от пользователя. Добавь контроллер, чтобы "вытащить" данные.
Пример базового добавления:

В переменных виджета:

```dart
TextEditingController _controller = TextEditingController();
```

В вёрстке виджета:

```dart
TextField(
    controller: _controller,
    decoration: InputDecoration(hintText: 'Введи что-то!'),
),
```

Чтение данных:

* Получение текста: `String input = _controller.text;`
* Для чисел: Необходимо преобразовать строку в число: `int num = int.parse(input);`;
* Для очистки: `_controller.clear();`

📌 Простой пример TextField

```dart
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: TextFieldExample(),
    );
  }
}

class TextFieldExample extends StatefulWidget {
  const TextFieldExample({super.key});

  @override
  State<TextFieldExample> createState() => _TextFieldExampleState();
}

class _TextFieldExampleState extends State<TextFieldExample> {
  final TextEditingController _controller = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: TextField(
          controller: _controller,
          decoration: const InputDecoration(
            hintText: 'Введите текст',
            border: OutlineInputBorder(),
          ),
        ),
      ),
    );
  }
}
```
---

## Кнопка: ElevatedButton

Запускает действие при нажатии.
Пример:

```dart
ElevatedButton(
    onPressed: () {
        // Твой Dart-код здесь!
        setState(() {
            // Обнови переменные для экрана
        });
    },
    child: Text('Нажми меня!'),
)
```

---

## setState

```dart
setState(() {
  // Обновляем переменные состояния здесь
  counter++;
});
```

**Как это работает:**
- Уведомляет Flutter об изменении состояния
- Вызывает перерисовку виджета с новыми данными
- Все изменения состояния должны быть внутри `setState`

> 💡 Без вызова `setState` изменения переменных не отобразятся на экране, даже если их значения изменились.

**[🏠 Вернуться на главную](README.md)**
