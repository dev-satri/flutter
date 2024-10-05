# Module 3: Working with Flutter Widgets

## 1. Building Custom Widgets

### 1.1 Composing Custom Widgets
Custom widgets allow you to create reusable UI components that can be composed to build complex interfaces. 

- **Creating a Custom Widget**:
  - You can create a custom widget by extending either `StatelessWidget` or `StatefulWidget`.

Example of a simple custom widget:
```
import 'package:flutter/material.dart';

class GreetingWidget extends StatelessWidget {
  final String name;

  GreetingWidget({required this.name});

  @override
  Widget build(BuildContext context) {
    return Text('Hello, $name!', style: TextStyle(fontSize: 24));
  }
}
```

### 1.2 Creating Reusable Components
Reusable components enhance your app's maintainability and consistency. You can create widgets that accept parameters to customize their behavior.

Example of a reusable button widget:
```
class CustomButton extends StatelessWidget {
  final String label;
  final VoidCallback onPressed;

  CustomButton({required this.label, required this.onPressed});

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      child: Text(label),
    );
  }
}
```

Usage example:
```
CustomButton(
  label: 'Click Me',
  onPressed: () {
    print('Button Pressed!');
  },
);
```

---

## 2. Navigation and Routing

### 2.1 Navigating between Screens
Flutter uses a navigation stack to manage transitions between screens. You can use the `Navigator` class to push new screens onto the stack.

Example of navigating to a new screen:
```
import 'package:flutter/material.dart';

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Home')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => SecondScreen()),
            );
          },
          child: Text('Go to Second Screen'),
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Second Screen')),
      body: Center(child: Text('Welcome to the Second Screen!')),
    );
  }
}
```

### 2.2 Passing Data between Screens
You can pass data to the next screen by including it in the constructor of the destination widget.

Example of passing data:
```
class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Home')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => SecondScreen(message: 'Hello from Home!'),
              ),
            );
          },
          child: Text('Go to Second Screen'),
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  final String message;

  SecondScreen({required this.message});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Second Screen')),
      body: Center(child: Text(message)),
    );
  }
}
```

### 2.3 Named Routes and Navigation Stack
Named routes simplify navigation by allowing you to define routes in a centralized manner.

Example of setting up named routes:
```
void main() {
  runApp(MaterialApp(
    initialRoute: '/',
    routes: {
      '/': (context) => HomeScreen(),
      '/second': (context) => SecondScreen(),
    },
  ));
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Home')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushNamed(context, '/second');
          },
          child: Text('Go to Second Screen'),
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Second Screen')),
      body: Center(child: Text('Welcome to the Second Screen!')),
    );
  }
}
```

---

## 3. Animations in Flutter

### 3.1 Basic Animations (Tween, AnimationController)
Flutter provides a rich set of animation features. The `AnimationController` class is used to control animations, while `Tween` defines the range of animation values.

Example of a basic animation:
```
class AnimationExample extends StatefulWidget {
  @override
  _AnimationExampleState createState() => _AnimationExampleState();
}

class _AnimationExampleState extends State<AnimationExample>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    );

    _animation = Tween<double>(begin: 0.0, end: 300.0).animate(_controller);
    _controller.forward();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _animation,
      builder: (context, child) {
        return Container(
          height: _animation.value,
          width: _animation.value,
          color: Colors.blue,
        );
      },
    );
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
}
```

### 3.2 Hero Animations and Page Transitions
Hero animations enable smooth transitions between screens. You can use the `Hero` widget to create a shared element transition.

Example of Hero animation:
```
class HeroExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Hero Animation')),
      body: Center(
        child: GestureDetector(
          onTap: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => SecondHeroScreen()),
            );
          },
          child: Hero(
            tag: 'hero-tag',
            child: Image.network('https://example.com/image.jpg'),
          ),
        ),
      ),
    );
  }
}

class SecondHeroScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Second Screen')),
      body: Center(
        child: Hero(
          tag: 'hero-tag',
          child: Image.network('https://example.com/image.jpg'),
        ),
      ),
    );
  }
}
```

### 3.3 Advanced Animations (Custom Animations)
Custom animations provide more control over the animation process. You can create complex animations by manipulating properties over time.

Example of a custom animation:
```
class CustomAnimationExample extends StatefulWidget {
  @override
  _CustomAnimationExampleState createState() => _CustomAnimationExampleState();
}

class _CustomAnimationExampleState extends State<CustomAnimationExample>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    )..repeat(reverse: true);
  }

  @override
  Widget build(BuildContext context) {
    return ScaleTransition(
      scale: Tween(begin: 0.0, end: 1.0).animate(_controller),
      child: FlutterLogo(size: 100),
    );
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
}
```

---

## 4. Handling Gestures

### 4.1 Gesture Detection (Tap, Swipe, Drag)
Flutter provides a set of gesture detectors to handle user interactions, such as taps, swipes, and drags.

Example of handling tap gestures:
```
class GestureExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        print('Tapped!');
      },
      child: Container(
        color: Colors.blue,
        width: 200,
        height: 200,
        child: Center(child: Text('Tap Me!', style: TextStyle(color: Colors.white))),
      ),
    );
  }
}
```

### 4.2 Using GestureDetector for Interactive UIs
The `GestureDetector` widget allows you to define multiple gesture handlers for a widget.

Example of handling multiple gestures:
```
class MultiGestureExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        print('Tapped!');
      },
      onDoubleTap: () {
        print('Double Tapped!');
      },
      onLongPress: () {
        print('Long Pressed!');
      },
      child: Container(
        color: Colors.green,
        width: 200,
        height: 200,
        child: Center(child: Text('Interact with Me!', style: TextStyle(color: Colors.white))),
      ),
    );
  }
}
```

---

## Assignments

1. **Create a Custom Widget**:
   - Design a custom widget that accepts parameters to display personalized content (e.g., a greeting message
