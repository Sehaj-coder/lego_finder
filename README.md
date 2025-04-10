# lego_finder
A Flutter app to identify and locate LEGO bricks using image recognition.
dependencies:
  flutter:
    sdk: flutter
  camera: ^0.10.5+2
  google_mlkit_image_labeling: ^0.4.0 # for image recognition
  path_provider: ^2.1.2
  path: ^1.8.3
flutter pub get
import 'package:flutter/material.dart';
import 'package:camera/camera.dart';

late List<CameraDescription> cameras;

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();
  cameras = await availableCameras();
  runApp(const LegoFinderApp());
}

class LegoFinderApp extends StatelessWidget {
  const LegoFinderApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'LEGO Brick Finder',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const CameraScreen(),
    );
  }
}

class CameraScreen extends StatefulWidget {
  const CameraScreen({super.key});

  @override
  State<CameraScreen> createState() => _CameraScreenState();
}

class _CameraScreenState extends State<CameraScreen> {
  late CameraController _controller;
  bool _isReady = false;

  @override
  void initState() {
    super.initState();
    _initializeCamera();
  }
  Future<void> _initializeCamera() async {
    _controller = CameraController(cameras[0], ResolutionPreset.high);
    await _controller.initialize();
    setState(() => _isReady = true);
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('LEGO Brick Finder')),
      body: _isReady
          ? CameraPreview(_controller)
          : const Center(child: CircularProgressIndicator()),
    );
  }
}
flutter run -d chrome
