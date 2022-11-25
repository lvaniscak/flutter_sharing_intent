# flutter_sharing_intent

A flutter plugin that allow flutter apps to receive photos, videos, text, urls or any other file types from another app.

### Also, iOS is not developed yet.

## Features

- It's allow to share image, text, video ,urls and file from other app to flutter app. 
- It's allow to share multiple image, multiple video and multiple file from other app to flutter app.
 
## Installing

command:

```dart
 $ flutter pub add flutter_sharing_intent
```

pubspec.yaml:

```dart
dependencies:
flutter_sharing_intent: ^(latest)
```

## Usage

## Full Example

```dart
import 'package:flutter/material.dart';
import 'dart:async';
import 'package:flutter_sharing_intent/flutter_sharing_intent.dart';
import 'package:flutter_sharing_intent/model/sharing_file.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({super.key});

  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  late StreamSubscription _intentDataStreamSubscription;
  List<SharedFile>? list;
  @override
  void initState() {
    super.initState();
    // For sharing images coming from outside the app while the app is in the memory
    _intentDataStreamSubscription = FlutterSharingIntent.instance.getMediaStream()
        .listen((List<SharedFile> value) {
      setState(() {
        list = value;
      });
      print("Shared: getMediaStream ${value.map((f) => f.value).join(",")}");
    }, onError: (err) {
      print("getIntentDataStream error: $err");
    });

    // For sharing images coming from outside the app while the app is closed
    FlutterSharingIntent.instance.getInitialSharing().then((List<SharedFile> value) {
      print("Shared: getInitialMedia ${value.map((f) => f.value).join(",")}");
      setState(() {
        list = value;
      });
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Plugin example app'),
        ),
        body: Center(
          child: Container(
              margin: EdgeInsets.symmetric(horizontal: 24),
              child: Text('Sharing data: \n${list?.join("\n\n")}\n')),
        ),
      ),
    );
  }
  @override
  void dispose() {
    _intentDataStreamSubscription.cancel();
    super.dispose();
  }
}
```

