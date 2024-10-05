# Module 2: Introduction to Flutter

## 1. Introduction to Flutter Framework

### 1.1 Overview of Flutter and Its Architecture
Flutter is an open-source UI software development toolkit created by Google. It allows developers to build natively compiled applications for mobile, web, and desktop from a single codebase.

#### Flutter's Architecture
- **Flutter Engine**: Written in C++, it provides low-level rendering support using Google's Skia graphics library.
- **Framework**: Written in Dart, it provides a rich set of widgets and APIs for building apps.
- **Embedder**: Platform-specific code that interacts with the underlying operating system.

### 1.2 Understanding Flutter Widgets
Widgets are the core building blocks of a Flutter application. Everything in Flutter is a widget, including layout models, UI components, and even the entire app.

- **Stateless Widgets**: Widgets that don’t store state (e.g., Text, Icon).
- **Stateful Widgets**: Widgets that hold state and can change (e.g., forms, user input).

### 1.3 Setting Up the Flutter Development Environment
1. Install Flutter SDK from the official website: [flutter.dev](https://flutter.dev)
2. Install IDE: Visual Studio Code or Android Studio.
3. Configure the Flutter and Dart plugins in your IDE.
4. Run the following command to check the setup:
   ```
   flutter doctor
   ```

### 1.4 Creating Your First Flutter App
1. Create a new Flutter project using:
   ```
   flutter create my_first_app
   ```
2. Open the project in your preferred IDE.
3. Run the app using:
   ```
   flutter run
   ```

---

## 2. Stateless and Stateful Widgets

### 2.1 Building Simple UIs with Stateless Widgets
A **StatelessWidget** is immutable. Once built, it cannot change its state.

Example:
```
class MyStatelessWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Text('Hello, Flutter!');
  }
}
```

### 2.2 Stateful Widgets for Dynamic UIs
**StatefulWidget** can change its state during runtime.

Example:
```
class MyStatefulWidget extends StatefulWidget {
  @override
  _MyStatefulWidgetState createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  String text = 'Hello, Flutter!';

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text(text),
        ElevatedButton(
          onPressed: () {
            setState(() {
              text = 'Hello, Stateful Widget!';
            });
          },
          child: Text('Change Text'),
        ),
      ],
    );
  }
}
```

### 2.3 Widget Lifecycle
- **initState()**: Called when the widget is inserted into the tree.
- **build()**: Called whenever the widget needs to be rendered.
- **dispose()**: Called when the widget is removed from the tree.

---

## 3. Flutter Layouts and Styling

### 3.1 Understanding Layout Widgets
- **Row**: Lays out children horizontally.
- **Column**: Lays out children vertically.
- **Stack**: Lays out children on top of each other.

Example (using a Column):
```
Column(
  children: <Widget>[
    Text('Item 1'),
    Text('Item 2'),
    Text('Item 3'),
  ],
)
```

### 3.2 Container, Padding, and Spacing Widgets
- **Container**: A widget that can be used to contain other widgets and apply styling.
- **Padding**: Adds space around a widget.
- **SizedBox**: Provides fixed spacing between widgets.

Example:
```
Container(
  padding: EdgeInsets.all(10),
  child: Text('Hello, Flutter!'),
)
```

### 3.3 Customizing Styles (Colors, Fonts, Themes)
Flutter allows customization of widget styles, including colors and fonts.

Example:
```
Text(
  'Styled Text',
  style: TextStyle(
    fontSize: 20,
    color: Colors.blue,
  ),
)
```

### 3.4 Responsive Design in Flutter
Flutter provides tools like `MediaQuery` and `LayoutBuilder` for creating responsive UIs.

Example (Responsive Text Size):
```
Text(
  'Responsive Text',
  style: TextStyle(
    fontSize: MediaQuery.of(context).size.width * 0.05,
  ),
)
```

---

## 4. Handling User Input

### 4.1 TextFields, Buttons, and Forms
Flutter provides various input widgets like `TextField` and `ElevatedButton`.

Example (TextField and Button):
```
Column(
  children: <Widget>[
    TextField(
      decoration: InputDecoration(
        labelText: 'Enter your name',
      ),
    ),
    ElevatedButton(
      onPressed: () {},
      child: Text('Submit'),
    ),
  ],
)
```

### 4.2 Form Validation
Flutter supports form validation by using `Form` and `FormField` widgets.

Example:
```
final _formKey = GlobalKey<FormState>();

Form(
  key: _formKey,
  child: Column(
    children: <Widget>[
      TextFormField(
        validator: (value) {
          if (value == null || value.isEmpty) {
            return 'Please enter some text';
          }
          return null;
        },
      ),
      ElevatedButton(
        onPressed: () {
          if (_formKey.currentState!.validate()) {
            // Process data
          }
        },
        child: Text('Submit'),
      ),
    ],
  ),
)
```

### 4.3 Managing State (setState, Blocs)
- **setState()**: Used for managing state within a widget.
- **Blocs (Business Logic Components)**: Provides a more scalable way to manage state across the app using reactive streams.

Example using `setState`:
```
setState(() {
  // Update state
});
```

Example using `Bloc`:
```
// Create a Bloc
class CounterBloc extends Bloc<CounterEvent, int> {
  CounterBloc() : super(0);

  @override
  Stream<int> mapEventToState(CounterEvent event) async* {
    if (event is IncrementEvent) {
      yield state + 1;
    }
  }
}

// Use Bloc in the UI
BlocBuilder<CounterBloc, int>(
  builder: (context, count) {
    return Text('$count');
  },
);
```
