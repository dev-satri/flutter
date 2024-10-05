# Module 1: Introduction to Dart Programming

## 1. Overview of Dart

### 1.1 Introduction to Dart and Its Ecosystem
Dart is a versatile, client-optimized programming language developed by Google. It is the language behind **Flutter**, which allows developers to build apps for multiple platforms (mobile, web, desktop) from a single codebase.

- **Why Dart?**
  - Dart provides fast execution because it can compile to native machine code.
  - It is designed for creating smooth and high-performance user interfaces, which is essential for modern apps.
  - Dart is easy to learn, especially for those familiar with JavaScript or other C-like languages.

- **Dart’s Key Features:**
  - **Strongly Typed**: Dart is statically typed, which helps catch errors at compile time.
  - **Garbage Collection**: Dart manages memory automatically, freeing you from worrying about it.
  - **Asynchronous Programming**: Dart has built-in support for asynchronous programming, which is crucial for building responsive apps.

- **The Dart Ecosystem:**
  - **Dart SDK**: Provides tools and libraries to develop Dart applications.
  - **Pub.dev**: The package manager for Dart and Flutter, used to manage dependencies and add functionality to your app.

### 1.2 Setting Up the Dart Development Environment
Follow these steps to set up Dart on your system:
1. **Install Dart SDK**: Download the Dart SDK from [dart.dev](https://dart.dev/get-dart).
2. **Install IDE**: Use a development environment like **Visual Studio Code** or **IntelliJ IDEA** to write Dart code. Both support Dart extensions/plugins for code completion, debugging, and more.
3. **Verify Installation**: Open a terminal and type:
   ```
   dart --version
   ```
   This should display the installed Dart version.

### 1.3 Dart Fundamentals (Data Types, Variables, Operators)
In Dart, variables store data, and each variable has a **data type**. Here are some basic data types:

- **int**: Stores whole numbers (e.g., 10, -5).
- **double**: Stores floating-point numbers (e.g., 3.14, -0.1).
- **String**: Stores text values (e.g., "Hello, World!").
- **bool**: Stores boolean values (true or false).
- **var**: Can store any data type; Dart automatically infers the type.

Example of basic Dart variables:
```
void main() {
  int age = 25;              // An integer
  double height = 5.9;        // A floating-point number
  String name = 'Alice';      // A string (text)
  bool isStudent = true;      // A boolean

  print('Name: $name, Age: $age, Height: $height, Student: $isStudent');
}
```

- **Operators**: Dart supports arithmetic (`+`, `-`, `*`, `/`), comparison (`==`, `!=`, `<`, `>`), and logical operators (`&&`, `||`, `!`).

---

## 2. Control Flow and Functions

### 2.1 Conditional Statements (if, else, switch)
Conditional statements help control the flow of a program by executing different code based on certain conditions.

- **if-else**: Use to execute different code blocks depending on a condition.
  Example:
  ```
  void main() {
    int score = 85;

    if (score >= 90) {
      print('Grade A');
    } else if (score >= 75) {
      print('Grade B');
    } else {
      print('Grade C');
    }
  }
  ```

- **switch**: Best for handling multiple fixed values. Use when you have many `if-else` conditions based on constant values.
  Example:
  ```
  void main() {
    String grade = 'A';

    switch (grade) {
      case 'A':
        print('Excellent');
        break;
      case 'B':
        print('Good');
        break;
      default:
        print('Needs Improvement');
    }
  }
  ```

### 2.2 Loops (for, while, do-while)
Loops allow you to execute a block of code multiple times.

- **for-loop**: Best used when you know how many times the loop will run.
  Example:
  ```
  void main() {
    for (int i = 1; i <= 5; i++) {
      print('Iteration $i');
    }
  }
  ```

- **while-loop**: Best when the number of iterations is not fixed and depends on a condition.
  Example:
  ```
  void main() {
    int count = 0;

    while (count < 3) {
      print('Count: $count');
      count++;
    }
  }
  ```

- **do-while**: Similar to `while`, but ensures the code runs at least once.
  Example:
  ```
  void main() {
    int i = 0;

    do {
      print('Count: $i');
      i++;
    } while (i < 3);
  }
  ```

### 2.3 Functions and Methods
Functions allow you to organize and reuse code. Dart supports both regular and anonymous (lambda) functions.

- **Defining a Function**:
  ```
  int add(int a, int b) {
    return a + b;
  }

  void main() {
    int sum = add(3, 5);
    print('Sum: $sum');
  }
  ```

- **Anonymous Functions (Lambda)**:
  ```
  var multiply = (int x, int y) => x * y;

  void main() {
    print(multiply(2, 3));  // Output: 6
  }
  ```

---

## 3. Object-Oriented Programming in Dart

### 3.1 Classes and Objects
Dart is an object-oriented language, meaning everything in Dart is an object, including numbers, strings, and even functions.

- **Defining a Class**:
  ```
  class Person {
    String name;
    int age;

    Person(this.name, this.age);

    void introduce() {
      print('Hello, I am $name and I am $age years old.');
    }
  }

  void main() {
    var person = Person('Alice', 25);
    person.introduce();  // Output: Hello, I am Alice and I am 25 years old.
  }
  ```

### 3.2 Constructors, Inheritance, and Polymorphism
- **Constructors**: Special methods used to initialize objects.
  - Dart provides a **default constructor** if you don’t define one. You can also create your own.
- **Inheritance**: A way for one class to acquire properties and methods of another class.
  - The subclass can override methods of the parent class.

Example of inheritance and method overriding:
```
class Animal {
  String name;

  Animal(this.name);

  void makeSound() {
    print('$name makes a sound.');
  }
}

class Dog extends Animal {
  Dog(String name) : super(name);

  @override
  void makeSound() {
    print('$name barks.');
  }
}

void main() {
  var dog = Dog('Buddy');
  dog.makeSound();  // Output: Buddy barks.
}
```

---

## 4. Error Handling in Dart

### 4.1 Exceptions and Try-Catch Blocks
- **Exceptions**: Dart uses exceptions to indicate errors or unexpected conditions. You can use `try-catch` blocks to handle exceptions and prevent program crashes.

Example:
```
void main() {
  try {
    int result = 10 ~/ 0;
    print(result);
  } catch (e) {
    print('Error: $e');  // Output: Error: IntegerDivisionByZeroException
  } finally {
    print('This code runs no matter what.');
  }
}
```

---

## Assignment Questions

### 1. Dart Fundamentals
- **Q1**: Write a program that stores your name, age, and favorite hobby in variables and prints them in a single sentence.
- **Q2**: Create a Dart program that performs arithmetic operations (addition, subtraction, multiplication, and division) on two numbers provided by the user.

### 2. Control Flow and Functions
- **Q1**: Write a program that checks whether a number entered by the user is positive, negative, or zero.
- **Q2**: Create a function that takes a person's name and age as input and returns a message like "Hi, I’m Alice and I’m 25 years old."

### 3. Object-Oriented Programming
- **Q1**: Create a `Car` class with properties `brand`, `model`, and `year`. Create a method `displayInfo()` that prints these details. Create objects of this class and call the method.
- **Q2**: Write a class `Animal` with a method `makeSound()`. Create subclasses `Cat` and `Bird` that override `makeSound()` to print appropriate animal sounds.

### 4. Error Handling
- **Q1**: Write a program that tries to divide a number by zero. Use a `try-catch` block to handle the division-by-zero exception.
- **Q2**: Create a custom exception `NegativeAgeException`. If a user enters
