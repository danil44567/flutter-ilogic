# Работа с API во Flutter

**[🏠 Вернуться на главную](README.md)**

## Содержание

1. [Введение в работу с API](#введение-в-работу-с-api)
2. [Добавление зависимости http](#добавление-зависимости-http)
3. [Выполнение GET-запроса](#выполнение-get-запроса)
4. [Обработка JSON-ответа](#обработка-json-ответа)
5. [Модель данных и преобразование JSON](#модель-данных-и-преобразование-json)
6. [Отображение данных на экране](#отображение-данных-на-экране)
7. [Обработка ошибок и состояний загрузки](#обработка-ошибок-и-состояний-загрузки)
8. [POST-запрос (отправка данных)](#post-запрос-отправка-данных)
9. [Практические задания](#практические-задания)

---

## Введение в работу с API

API (Application Programming Interface) — это способ получения данных из интернета. Во Flutter для работы с сетевыми запросами чаще всего используют пакет **http**.

С его помощью можно:
- Получать данные (GET-запрос)
- Отправлять данные (POST-запрос)
- Обновлять или удалять данные (PUT, DELETE)

В этом уроке мы научимся делать простые запросы к публичным API, парсить JSON и отображать полученные данные в приложении.

---

## Добавление зависимости http

Откройте файл `pubspec.yaml` в корне проекта и добавьте зависимость:

```yaml
dependencies:
  flutter:
    sdk: flutter
  http: ^1.2.2  # актуальную версию можно посмотреть на pub.dev
```

После сохранения выполните в терминале:

```bash
flutter pub get
```

---

## Выполнение GET-запроса

Для простоты будем использовать публичное тестовое API:  
**https://jsonplaceholder.typicode.com/posts** — возвращает список постов.

Пример базового GET-запроса:

```dart
import 'package:http/http.dart' as http;

Future<void> fetchPosts() async {
  final response = await http.get(
    Uri.parse('https://jsonplaceholder.typicode.com/posts'),
  );

  if (response.statusCode == 200) {
    print(response.body); // JSON в виде строки
  } else {
    print('Ошибка: ${response.statusCode}');
  }
}
```

---

## Обработка JSON-ответа

Ответ от сервера приходит в виде строки JSON. Для преобразования в Dart-объекты используем `dart:convert`.

```dart
import 'dart:convert';

List<dynamic> posts = jsonDecode(response.body);
```

`jsonDecode` возвращает `List` или `Map` в зависимости от структуры данных.

---

## Модель данных и преобразование JSON

Чтобы удобно работать с данными, создадим класс-модель.

Пример поста из JSONPlaceholder:

```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere...",
  "body": "quia et suscipit\nsuscipit recusandae..."
}
```

Создадим класс `Post`:

```dart
class Post {
  final int userId;
  final int id;
  final String title;
  final String body;

  Post({
    required this.userId,
    required this.id,
    required this.title,
    required this.body,
  });

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      userId: json['userId'] as int,
      id: json['id'] as int,
      title: json['title'] as String,
      body: json['body'] as String,
    );
  }
}
```

Теперь можно преобразовать список:

```dart
List<Post> parsePosts(String responseBody) {
  final parsed = jsonDecode(responseBody).cast<Map<String, dynamic>>();
  return parsed.map<Post>((json) => Post.fromJson(json)).toList();
}
```

---

## Отображение данных на экране

Используем `FutureBuilder` для асинхронной загрузки данных.

```dart
class PostsPage extends StatelessWidget {
  const PostsPage({super.key});

  Future<List<Post>> fetchPosts() async {
    final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));
    
    if (response.statusCode == 200) {
      return parsePosts(response.body);
    } else {
      throw Exception('Не удалось загрузить посты');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Посты')),
      body: FutureBuilder<List<Post>>(
        future: fetchPosts(),
        builder: (context, snapshot) {
          if (snapshot.hasData) {
            return ListView.builder(
              itemCount: snapshot.data!.length,
              itemBuilder: (context, index) {
                final post = snapshot.data![index];
                return ListTile(
                  title: Text(post.title),
                  subtitle: Text(post.body),
                );
              },
            );
          } else if (snapshot.hasError) {
            return Center(child: Text('Ошибка: ${snapshot.error}'));
          }
          return const Center(child: CircularProgressIndicator());
        },
      ),
    );
  }
}
```

---

## Обработка ошибок и состояний загрузки

Всегда учитывайте три состояния:
- Загрузка (`CircularProgressIndicator`)
- Успешно получены данные
- Ошибка (нет интернета, сервер вернул ошибку)

---

## POST-запрос (отправка данных)

Пример отправки нового поста:

```dart
Future<Post> createPost(String title, String body) async {
  final response = await http.post(
    Uri.parse('https://jsonplaceholder.typicode.com/posts'),
    headers: {'Content-Type': 'application/json'},
    body: jsonEncode({
      'title': title,
      'body': body,
      'userId': 1,
    }),
  );

  if (response.statusCode == 201) {
    return Post.fromJson(jsonDecode(response.body));
  } else {
    throw Exception('Не удалось создать пост');
  }
}
```

**[🏠 Вернуться на главную](README.md)**
