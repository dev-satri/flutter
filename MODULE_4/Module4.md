# Module 4: Networking and Backend Integration

## 1. HTTP Requests and REST API Integration

### 1.1 Fetching Data from APIs using http package
To interact with REST APIs in Flutter, you can use the `http` package. This package provides functions to send HTTP requests and receive responses.

- **Adding Dependency**:
  Add the `http` package to your `pubspec.yaml` file:
  ```
  dependencies:
    http: ^0.14.0
  ```

- **Fetching Data Example**:
  The following example demonstrates how to make a GET request to fetch data from a public API.

```
import 'package:http/http.dart' as http;
import 'dart:convert';

Future<void> fetchData() async {
  final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));

  if (response.statusCode == 200) {
    List<dynamic> data = json.decode(response.body);
    print(data);
  } else {
    throw Exception('Failed to load data');
  }
}
```

### 1.2 Parsing JSON Data
Once you receive the data, you often need to parse the JSON into Dart objects. You can use the `dart:convert` library to decode JSON strings.

- **Parsing JSON Example**:
  Here’s how you can define a model class and parse JSON data:

```
class Post {
  final int id;
  final String title;
  final String body;

  Post({required this.id, required this.title, required this.body});

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      id: json['id'],
      title: json['title'],
      body: json['body'],
    );
  }
}

Future<List<Post>> fetchPosts() async {
  final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));

  if (response.statusCode == 200) {
    List<dynamic> jsonData = json.decode(response.body);
    return jsonData.map((data) => Post.fromJson(data)).toList();
  } else {
    throw Exception('Failed to load posts');
  }
}
```

### 1.3 Handling API Responses and Errors
It's crucial to handle different API responses and potential errors gracefully.

- **Handling Responses Example**:
  You can handle the response using a try-catch block to manage exceptions.

```
Future<void> fetchDataWithErrorHandling() async {
  try {
    final response = await http.get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));

    if (response.statusCode == 200) {
      List<Post> posts = fetchPosts();
      print(posts);
    } else {
      print('Error: ${response.statusCode}');
    }
  } catch (e) {
    print('Error fetching data: $e');
  }
}
```

---

## 2. State Management Techniques

### 2.1 Provider, Riverpod, and GetX
State management is crucial for maintaining application state across various parts of your app. Several popular packages are used for this purpose, including Provider, Riverpod, and GetX.

- **Using Provider Example**:
  Here’s a simple example of using Provider for state management.

```
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

class Counter with ChangeNotifier {
  int _count = 0;

  int get count => _count;

  void increment() {
    _count++;
    notifyListeners();
  }
}

void main() {
  runApp(
    ChangeNotifierProvider(
      create: (context) => Counter(),
      child: MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Provider Example')),
        body: Center(
          child: Consumer<Counter>(
            builder: (context, counter, child) {
              return Text('Count: ${counter.count}');
            },
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {
            Provider.of<Counter>(context, listen: false).increment();
          },
          child: Icon(Icons.add),
        ),
      ),
    );
  }
}
```

### 2.2 Managing Global State and Local State
Understanding the difference between global and local state is essential. Use global state management for app-wide states, while local state is specific to a widget.

- **Example**:
  Use Provider for global state and `setState` for local state management.

### 2.3 Understanding the BLoC Pattern
BLoC (Business Logic Component) is a design pattern that separates business logic from UI. It uses Streams and Sinks to manage state and events.

- **BLoC Example**:
  Here’s a simple BLoC implementation.

```
import 'dart:async';

class CounterBloc {
  int _count = 0;
  final _countController = StreamController<int>();

  Stream<int> get countStream => _countController.stream;

  void increment() {
    _count++;
    _countController.sink.add(_count);
  }

  void dispose() {
    _countController.close();
  }
}
```

---

## 3. Persistent Storage in Flutter

### 3.1 Local Database with SQLite/Drift
SQLite is a popular choice for local storage in mobile applications. You can use the `sqflite` package for SQLite integration.

- **Adding Dependency**:
  Add `sqflite` and `path` to your `pubspec.yaml`:
  ```
  dependencies:
    sqflite: ^2.0.0
    path: ^1.8.0
  ```

- **Example of Using SQLite**:
  Here’s a simple SQLite example to create a table and insert data.

```
import 'package:sqflite/sqflite.dart';
import 'package:path/path.dart';

Future<Database> openDatabaseConnection() async {
  return await openDatabase(
    join(await getDatabasesPath(), 'my_database.db'),
    onCreate: (db, version) {
      return db.execute(
        'CREATE TABLE notes(id INTEGER PRIMARY KEY, content TEXT)',
      );
    },
    version: 1,
  );
}

Future<void> insertNote(String content) async {
  final db = await openDatabaseConnection();
  await db.insert(
    'notes',
    {'content': content},
    conflictAlgorithm: ConflictAlgorithm.replace,
  );
}
```

### 3.2 Using SharedPreferences and GetStorage
SharedPreferences is a simple key-value storage solution for storing lightweight data. `GetStorage` is another alternative for persistent storage.

- **Using SharedPreferences Example**:
  To store simple data:
  
```
import 'package:shared_preferences/shared_preferences.dart';

Future<void> saveCounter(int count) async {
  final prefs = await SharedPreferences.getInstance();
  await prefs.setInt('counter', count);
}

Future<int> getCounter() async {
  final prefs = await SharedPreferences.getInstance();
  return prefs.getInt('counter') ?? 0;
}
```

### 3.3 Working with Files and Directories
For more complex data, you might need to read and write files. Use the `path_provider` package to get the correct directories.

- **Example of File Handling**:
  Here’s how to read and write to a file.

```
import 'dart:io';
import 'package:path_provider/path_provider.dart';

Future<String> getFilePath() async {
  final directory = await getApplicationDocumentsDirectory();
  return '${directory.path}/my_file.txt';
}

Future<void> writeToFile(String content) async {
  final path = await getFilePath();
  final file = File(path);
  await file.writeAsString(content);
}

Future<String> readFromFile() async {
  try {
    final path = await getFilePath();
    final file = File(path);
    return await file.readAsString();
  } catch (e) {
    return 'Error reading file: $e';
  }
}
```

---

## Assignments

1. **API Data Fetching**:
   - Create a Flutter app that fetches data from a public API and displays it in a list format.

2. **JSON Parsing**:
   - Extend the previous assignment to parse the JSON data into Dart objects and display specific fields (e.g., title and body).

3. **State Management**:
   - Implement a simple counter app using Provider, where tapping a button increments the counter and updates the UI.

4. **BLoC Pattern**:
   - Create a Flutter app that uses the BLoC pattern to manage a counter. Implement a UI that displays the count and buttons to increment or decrement it.

5. **SQLite Integration**:
   - Develop a notes app that allows users to add, view, and delete notes using SQLite as the local database.

6. **Persistent Storage**:
   - Create a Flutter app that uses SharedPreferences to save user preferences (e.g., theme selection) and retrieve them on startup.
