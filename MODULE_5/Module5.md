# Module 5: Advanced Flutter Concepts

## 1. Firebase Integration

### 1.1 Setting Up Firebase in a Flutter App
Firebase provides various tools and services for mobile app development. To integrate Firebase with your Flutter app, follow these steps:

- **Create a Firebase Project**:
  1. Go to the [Firebase Console](https://console.firebase.google.com/).
  2. Click on "Add project" and follow the prompts to create a new project.

- **Add Firebase to your Flutter App**:
  1. Click on "Add app" and choose Android/iOS.
  2. Follow the instructions to download the `google-services.json` (for Android) or `GoogleService-Info.plist` (for iOS) and add them to your project.

- **Add Dependencies**:
  Add necessary Firebase dependencies in your `pubspec.yaml`:

  ```
  dependencies:
    firebase_core: ^2.1.0
    firebase_auth: ^5.0.0
    cloud_firestore: ^3.1.0
  ```

### 1.2 Firebase Authentication (Email, Google Sign-In)
Firebase Authentication provides backend services to authenticate users.

- **Email/Password Authentication Example**:

```
import 'package:firebase_auth/firebase_auth.dart';

Future<void> signUp(String email, String password) async {
  try {
    UserCredential userCredential = await FirebaseAuth.instance.createUserWithEmailAndPassword(
      email: email,
      password: password,
    );
    print('User registered: ${userCredential.user?.uid}');
  } catch (e) {
    print('Failed to register: $e');
  }
}

Future<void> signIn(String email, String password) async {
  try {
    UserCredential userCredential = await FirebaseAuth.instance.signInWithEmailAndPassword(
      email: email,
      password: password,
    );
    print('User signed in: ${userCredential.user?.uid}');
  } catch (e) {
    print('Failed to sign in: $e');
  }
}
```

- **Google Sign-In Example**:
  To use Google Sign-In, you need to configure the Google sign-in method in Firebase Console.

```
import 'package:google_sign_in/google_sign_in.dart';

final GoogleSignIn googleSignIn = GoogleSignIn();

Future<void> signInWithGoogle() async {
  try {
    final GoogleSignInAccount? googleUser = await googleSignIn.signIn();
    final GoogleSignInAuthentication googleAuth = await googleUser!.authentication;

    final AuthCredential credential = GoogleAuthProvider.credential(
      accessToken: googleAuth.accessToken,
      idToken: googleAuth.idToken,
    );

    await FirebaseAuth.instance.signInWithCredential(credential);
    print('User signed in with Google: ${googleUser.email}');
  } catch (e) {
    print('Failed to sign in with Google: $e');
  }
}
```

### 1.3 Firebase Firestore for Realtime Data
Cloud Firestore is a flexible, scalable database for mobile, web, and server development.

- **Setting Up Firestore**:
  Ensure that you have added `cloud_firestore` in your `pubspec.yaml` as mentioned earlier.

- **Firestore Example**:
  Here’s how to add, read, and listen to Firestore data.

```
import 'package:cloud_firestore/cloud_firestore.dart';

final FirebaseFirestore firestore = FirebaseFirestore.instance;

// Adding Data
Future<void> addData() async {
  await firestore.collection('users').add({
    'name': 'John Doe',
    'age': 25,
  });
}

// Reading Data
Future<void> getData() async {
  QuerySnapshot querySnapshot = await firestore.collection('users').get();
  for (var doc in querySnapshot.docs) {
    print('User: ${doc['name']}, Age: ${doc['age']}');
  }
}

// Listening to Data Changes
void listenToData() {
  firestore.collection('users').snapshots().listen((snapshot) {
    for (var doc in snapshot.docs) {
      print('User: ${doc['name']}, Age: ${doc['age']}');
    }
  });
}
```

### 1.4 Firebase Cloud Messaging (Push Notifications)
Firebase Cloud Messaging (FCM) allows you to send notifications to your users.

- **Setting Up FCM**:
  1. Enable Cloud Messaging in the Firebase Console.
  2. Add the required dependencies in `pubspec.yaml`:

  ```
  dependencies:
    firebase_messaging: ^14.0.0
  ```

- **FCM Example**:
  Here’s a simple example to initialize Firebase Messaging.

```
import 'package:firebase_messaging/firebase_messaging.dart';

Future<void> setupFirebaseMessaging() async {
  FirebaseMessaging messaging = FirebaseMessaging.instance;

  // Request permission
  NotificationSettings settings = await messaging.requestPermission();

  // Get the token
  String? token = await messaging.getToken();
  print('FCM Token: $token');

  // Handle background messages
  FirebaseMessaging.onBackgroundMessage(_firebaseMessagingBackgroundHandler);
}

Future<void> _firebaseMessagingBackgroundHandler(RemoteMessage message) async {
  print('Handling a background message: ${message.messageId}');
}
```

---

## 2. Flutter and Device Features

### 2.1 Accessing Device Sensors (Camera, Location)
Flutter provides plugins to access device features like camera and location.

- **Camera Example**:
  To use the camera, add the `camera` dependency to your `pubspec.yaml`:

  ```
  dependencies:
    camera: ^0.10.0
  ```

  - **Using the Camera**:
  Here’s how to implement camera functionality:

```
import 'package:camera/camera.dart';

Future<void> initializeCamera() async {
  // Get a list of available cameras
  final cameras = await availableCameras();
  final firstCamera = cameras.first;

  // Initialize the camera controller
  CameraController controller = CameraController(firstCamera, ResolutionPreset.high);
  await controller.initialize();
  // Add code to display the camera preview and take pictures
}
```

- **Location Example**:
  To use the location feature, add the `geolocator` package:

  ```
  dependencies:
    geolocator: ^8.2.0
  ```

```
import 'package:geolocator/geolocator.dart';

Future<void> getCurrentLocation() async {
  LocationPermission permission = await Geolocator.checkPermission();
  if (permission == LocationPermission.denied) {
    permission = await Geolocator.requestPermission();
  }

  if (permission == LocationPermission.always || permission == LocationPermission.whileInUse) {
    Position position = await Geolocator.getCurrentPosition(desiredAccuracy: LocationAccuracy.high);
    print('Current Location: ${position.latitude}, ${position.longitude}');
  } else {
    print('Location permissions are denied');
  }
}
```

### 2.2 Using Permissions (permission_handler package)
Managing permissions is crucial in Flutter apps. You can use the `permission_handler` package.

- **Add Dependency**:
  ```
  dependencies:
    permission_handler: ^10.0.0
  ```

- **Requesting Permissions Example**:

```
import 'package:permission_handler/permission_handler.dart';

Future<void> requestPermission() async {
  var status = await Permission.camera.status;
  if (!status.isGranted) {
    await Permission.camera.request();
  }
}
```

### 2.3 Handling File Uploads and Downloads
Flutter allows you to upload and download files through various plugins.

- **File Upload Example**:
  To upload files, you can use the `dio` package.

```
import 'package:dio/dio.dart';

Future<void> uploadFile(String filePath) async {
  FormData formData = FormData.fromMap({
    "file": await MultipartFile.fromFile(filePath, filename: "upload.txt"),
  });

  var response = await Dio().post('https://example.com/upload', data: formData);
  print(response.data);
}
```

- **File Download Example**:

```
Future<void> downloadFile(String url) async {
  var response = await Dio().download(url, 'path/to/save/file.txt');
  print('Download complete: ${response.statusCode}');
}
```

---

## 3. Platform-Specific Code

### 3.1 Writing Platform-Specific Code for iOS and Android
In Flutter, you can write platform-specific code using conditional imports.

- **Example of Platform-Specific Code**:
You can use `Platform` to check the current platform:

```
import 'dart:io';

void performPlatformSpecificTask() {
  if (Platform.isAndroid) {
    // Android-specific code
    print('Running on Android');
  } else if (Platform.isIOS) {
    // iOS-specific code
    print('Running on iOS');
  }
}
```

### 3.2 Using Method Channels to Integrate Native Code
Method channels allow you to invoke native code from Flutter.

- **Creating a Method Channel**:

```
import 'package:flutter/services.dart';

class NativeBridge {
  static const platform = MethodChannel('com.example/native');

  Future<String> getNativeData() async {
    try {
      final String result = await platform.invokeMethod('getNativeData');
      return result;
    } on PlatformException catch (e) {
      print('Failed to get native data: ${e.message}');
      return 'Error';
    }
  }
}
```

- **Implementing Native Code**:
  On the Android side (Java/Kotlin) or iOS side (Swift/Objective-C), implement the method to handle the call from Flutter.

---

## Assignment Questions

1. **Firebase Authentication**:
   - Create a Flutter app that allows users to register and log in using email and password authentication.
   - Implement Google Sign-In functionality.

2. **Firestore**:
   - Build a simple app that stores user profiles in Firestore. Each profile should include a name and age, and display all profiles in a list.

3. **Push Notifications**:
   - Set up Firebase Cloud Messaging in your app and implement a feature to send push notifications.

4. **Device Features**:
   - Create an app that takes a picture using the camera and displays it on the screen. Also, implement location fetching and display the current coordinates.

5. **Permissions**:
   - Implement a feature that requests camera permissions before taking a photo.

6. **File Handling**:
   - Build a simple app that allows users to upload a text file and display its content in the app.

7. **Platform-Specific Code**:
   - Create a method to show different messages for Android and iOS platforms when a button is pressed.

8. **Method Channels**:
   - Set up a method channel to fetch data from native Android/iOS code and display it in your Flutter app.
