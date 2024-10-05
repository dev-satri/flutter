# Module 2: Introduction to Flutter

## 1. Introduction to Flutter Framework

### 1.1 Overview of Flutter and Its Architecture
**Flutter** is an open-source UI software development toolkit created by Google. It allows developers to build natively compiled applications for mobile, web, and desktop from a single codebase.

- **Key Features of Flutter**:
  - **Hot Reload**: Allows developers to see changes in the app in real-time without restarting it.
  - **Widgets**: Everything in Flutter is a widget. Widgets are reusable components that represent UI elements.
  - **High Performance**: Flutter compiles to native ARM code, which results in excellent performance.

- **Flutter Architecture**:
  - **Dart Platform**: Flutter apps are written in the Dart programming language.
  - **Flutter Engine**: Renders widgets and handles low-level tasks such as graphics, text, and file I/O.
  - **Foundation Library**: Provides the core Flutter functionality, including APIs for animation, gestures, and more.
  - **Widgets**: The UI components built on top of the Flutter framework.

### 1.2 Understanding Flutter Widgets
In Flutter, everything is a widget, from buttons to padding. Widgets are the building blocks of a Flutter app.

- **Types of Widgets**:
  - **Stateless Widgets**: Immutable widgets that do not change their state once built. They are used for static UI.
  - **Stateful Widgets**: Mutable widgets that can change their state during the app’s lifecycle.

Example of a Stateless Widget:
```
import 'package:flutter/material.dart';

class MyStatelessWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Text('Hello, Flutter!');
  }
}
```

Example of a Stateful Widget:
```
import 'package:flutter/material.dart';

class MyStatefulWidget extends StatefulWidget {
  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Counter: $_counter'),
        ElevatedButton(
          onPressed: _incrementCounter,
          child: Text('Increment'),
        ),
      ],
    );
  }
}
```

### 1.3 Setting Up the Flutter Development Environment
To develop Flutter applications, follow these steps to set up your environment:

1. **Install Flutter SDK**:
   - Download the Flutter SDK from the [Flutter official website](https://flutter.dev/docs/get-started/install).

2. **Install an IDE**:
   - Recommended IDEs: **Visual Studio Code** or **Android Studio**.
   - Install the Flutter and Dart plugins for your chosen IDE.

3. **Set Up an Emulator or Physical Device**:
   - For Android, set up an Android Virtual Device (AVD) using Android Studio.
   - For iOS, use Xcode on macOS to create an iOS simulator.

4. **Verify the Installation**:
   - Open a terminal and run:
   ```
   flutter doctor
   ```
   This command checks your environment and displays any missing dependencies.

### 1.4 Creating Your First Flutter App
To create a basic Flutter app, follow these steps:

1. **Create a New Flutter Project**:
   ```
   flutter create my_first_app
   ```

2. **Navigate to the Project Directory**:
   ```
   cd my_first_app
   ```

3. **Run the App**:
   ```
   flutter run
   ```

This will launch the default Flutter app, which displays a simple counter.

---

## 2. Stateless and Stateful Widgets

### 2.1 Building Simple UIs with Stateless Widgets
Stateless widgets are ideal for static UI components that do not require dynamic content.

Example of a simple UI using Stateless Widgets:
```
import 'package:flutter/material.dart';

class MySimpleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('My Simple App')),
        body: Center(child: Text('Welcome to Flutter!')),
      ),
    );
  }
}

void main() {
  runApp(MySimpleApp());
}
```

### 2.2 Stateful Widgets for Dynamic UIs
Stateful widgets are used when the UI can change dynamically based on user interaction or data changes.

Example of a stateful widget:
```
import 'package:flutter/material.dart';

class CounterApp extends StatefulWidget {
  @override
  _CounterAppState createState() => _CounterAppState();
}

class _CounterAppState extends State<CounterApp> {
  int _count = 0;

  void _incrementCount() {
    setState(() {
      _count++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Counter')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Count: $_count'),
            ElevatedButton(
              onPressed: _incrementCount,
              child: Text('Increment'),
            ),
          ],
        ),
      ),
    );
  }
}

void main() {
  runApp(MaterialApp(home: CounterApp()));
}
```

### 2.3 Widget Lifecycle
Stateful widgets have a lifecycle that consists of various stages, including:
- **initState**: Called when the widget is inserted into the widget tree. This is where you can initialize state.
- **build**: Called whenever the widget needs to be drawn.
- **didChangeDependencies**: Called when the dependencies change.
- **setState**: Called to update the UI when the state changes.
- **dispose**: Called when the widget is removed from the widget tree. Cleanup resources here.

Example of using `initState`:
```
class MyWidget extends StatefulWidget {
  @override
  _MyWidgetState createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  @override
  void initState() {
    super.initState();
    // Initialization code here
  }

  @override
  Widget build(BuildContext context) {
    return Container(); // Your widget UI here
  }
}
```

---

## 3. Flutter Layouts and Styling

### 3.1 Understanding Layout Widgets (Row, Column, Stack)
Flutter provides various layout widgets to organize your UI:

- **Row**: Arranges its children in a horizontal line.
- **Column**: Arranges its children in a vertical line.
- **Stack**: Allows you to overlay widgets on top of each other.

Example of using Row and Column:
```
class LayoutExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Row(
          children: [
            Text('Item 1'),
            Text('Item 2'),
          ],
        ),
        Row(
          mainAxisAlignment: MainAxisAlignment.spaceAround,
          children: [
            Text('Item 3'),
            Text('Item 4'),
          ],
        ),
      ],
    );
  }
}
```

### 3.2 Container, Padding, and Spacing Widgets
- **Container**: A versatile widget for drawing boxes with customizable properties such as padding, margin, decoration, and constraints.
- **Padding**: Adds space around a widget.
- **SizedBox**: A box with a specified size, useful for adding space between widgets.

Example of using Container and Padding:
```
class PaddingExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(16.0),
      child: Column(
        children: [
          Text('Hello'),
          SizedBox(height: 10),
          Text('Flutter!'),
        ],
      ),
    );
  }
}
```

### 3.3 Customizing Styles (Colors, Fonts, Themes)
You can customize the appearance of your widgets using styles. Flutter supports various color, font, and theme options.

Example of changing the theme:
```
class ThemedApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
        primarySwatch: Colors.blue,
        textTheme: TextTheme(bodyText1: TextStyle(color: Colors.blue)),
      ),
      home: Scaffold(appBar: AppBar(title: Text('Themed App'))),
    );
  }
}
```

### 3.4 Responsive Design in Flutter
Responsive design is crucial for apps that run on different screen sizes. Use `MediaQuery` to get information about the device’s size.

Example of responsive layout:
```
class ResponsiveLayout extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var screenWidth = MediaQuery.of(context).size.width;

    return Container(
      width: screenWidth > 600 ? 600 : screenWidth, // Responsive width
      child: Column(
        children: [Text('Responsive Design')],
      ),
    );
  }
}
```

---

## 4. Handling User Input

### 4.1 TextFields, Buttons, and Forms
User input can be captured using `TextField` for text input and `ElevatedButton` for button interactions.

Example of a TextField and Button:
```
class InputExample extends StatelessWidget {
  final TextEditingController _controller = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        TextField(controller: _controller),
        ElevatedButton(
          onPressed: () {
            print(_controller.text);
          },
          child: Text('Submit'),
        ),
      ],
    );
  }
}
```

### 4.2 Form Validation
Forms are essential for capturing user input and validating it. Use the `Form` widget to group multiple form fields.

Example of form validation:
```
class FormExample extends StatefulWidget {
  @override
  _FormExampleState createState() => _FormExampleState();
}

class _FormExampleState extends State<FormExample> {
  final _formKey = GlobalKey<FormState>();
  final TextEditingController _controller = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Form(
      key: _formKey,
      child: Column(
        children: [
          TextFormField(
            controller: _controller,
            validator: (value) {
              if (value == null || value.isEmpty) {
                return 'Please enter some text';
              }
              return null; // Return null if the input is valid
            },
          ),
          ElevatedButton(
            onPressed: () {
              if (_formKey.currentState!.validate()) {
                print('Valid Input: ${_controller.text}');
              }
            },
            child: Text('Submit'),
          ),
        ],
      ),
    );
  }
}
```

### 4.3 Managing State (setState, Blocs)
State management is crucial in Flutter for handling dynamic UI changes. The most basic way to manage state is through the `setState` method in stateful widgets.

Example of managing state:
```
class StateManagementExample extends StatefulWidget {
  @override
  _StateManagementExampleState createState() => _StateManagementExampleState();
}

class _StateManagementExampleState extends State<StateManagementExample> {
  int _count = 0;

  void _incrementCount() {
    setState(() {
      _count++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Count: $_count'),
        ElevatedButton(
          onPressed: _incrementCount,
          child: Text('Increment'),
        ),
      ],
    );
  }
}
```

---

## Assignments

1. **Create a Simple Flutter App**:
   - Set up a Flutter environment and create a basic Flutter app displaying a welcome message.

2. **Build a Counter App**:
   - Develop a stateful widget that includes a counter. Implement buttons to increase and decrease the count.

3. **Design a Responsive Layout**:
   - Create a responsive layout that adjusts its size and orientation based on the screen size. Use MediaQuery to determine screen dimensions.

4. **Implement a Form with Validation**:
   - Create a form with at least two text fields and implement validation for both fields. Ensure users cannot submit the form unless all fields are filled out correctly.

5. **Explore Widgets**:
   - Experiment with different layout widgets (Row, Column, Stack) and style them with padding, colors, and themes. Document the results.

6. **Use the Bloc Pattern**:
   - Research the Bloc pattern and implement a simple example to manage state in your Flutter app.
