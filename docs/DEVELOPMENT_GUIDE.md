# Development Guide

## Table of Contents

- [Development Environment Setup](#development-environment-setup)
- [Project Architecture](#project-architecture)
- [Coding Standards](#coding-standards)
- [Development Workflow](#development-workflow)
- [Testing Guidelines](#testing-guidelines)
- [Performance Optimization](#performance-optimization)
- [Debugging](#debugging)
- [Deployment](#deployment)
- [Common Issues](#common-issues)

## Development Environment Setup

### Prerequisites

1. **Flutter SDK Installation**
   ```bash
   # Download Flutter SDK
   git clone https://github.com/flutter/flutter.git -b stable
   
   # Add to PATH
   export PATH="$PATH:`pwd`/flutter/bin"
   
   # Verify installation
   flutter doctor
   ```

2. **IDE Setup**
   
   **VS Code (Recommended)**:
   - Install Flutter extension
   - Install Dart extension
   - Install Flutter Widget Snippets
   
   **Android Studio**:
   - Install Flutter plugin
   - Install Dart plugin

3. **Device Setup**
   
   **Android**:
   ```bash
   # Install Android SDK
   flutter doctor --android-licenses
   
   # Create virtual device
   flutter emulators --create
   ```
   
   **iOS** (macOS only):
   ```bash
   # Install Xcode
   sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
   
   # Install iOS simulator
   open -a Simulator
   ```

### Project Setup

1. **Clone and Setup**
   ```bash
   git clone <repository-url>
   cd food-delivery-app
   flutter pub get
   ```

2. **Verify Setup**
   ```bash
   flutter doctor -v
   flutter run
   ```

## Project Architecture

### Directory Structure

```
lib/
├── main.dart              # App entry point
├── screens/               # UI screens
│   └── home.dart         # Home screen implementation
├── widgets/              # Reusable widgets (future)
├── models/               # Data models (future)
├── services/             # API services (future)
├── utils/                # Utility functions (future)
└── constants/            # App constants (future)
```

### Architecture Pattern

The current app follows a **Simple Widget-based Architecture**:
- **Presentation Layer**: Widgets and Screens
- **State Management**: StatelessWidget (current), StatefulWidget (future)
- **Data Layer**: Local assets (current), API integration (future)

### Key Components

1. **MyApp**: Root application widget
2. **HomeScreen**: Main screen with food listings
3. **UI Components**: Card widgets, gradient containers
4. **Assets**: Food images and icons

## Coding Standards

### Dart Style Guide

Follow the [Effective Dart](https://dart.dev/guides/language/effective-dart) guidelines:

1. **Naming Conventions**
   ```dart
   // Classes: PascalCase
   class HomeScreen extends StatelessWidget {}
   
   // Variables: camelCase
   String userName = 'John Doe';
   
   // Constants: camelCase with const
   const String appTitle = 'Food Delivery';
   
   // Private members: _camelCase
   Widget _buildCard() {}
   ```

2. **File Organization**
   ```dart
   // Import order: dart, flutter, packages, local
   import 'dart:io';
   import 'package:flutter/material.dart';
   import 'package:http/http.dart';
   import 'screens/home.dart';
   ```

3. **Code Structure**
   ```dart
   class ExampleWidget extends StatelessWidget {
     // 1. Constants
     static const String routeName = '/example';
     
     // 2. Properties
     final String title;
     
     // 3. Constructor
     const ExampleWidget({Key? key, required this.title}) : super(key: key);
     
     // 4. Build method
     @override
     Widget build(BuildContext context) {
       return Container();
     }
     
     // 5. Private methods
     Widget _buildHelper() {}
   }
   ```

### Widget Guidelines

1. **Widget Composition**
   ```dart
   // Good: Break down complex widgets
   Widget build(BuildContext context) {
     return Scaffold(
       appBar: _buildAppBar(),
       body: _buildBody(),
     );
   }
   
   Widget _buildAppBar() => AppBar(title: Text('Food App'));
   Widget _buildBody() => Container(child: _buildContent());
   ```

2. **Responsive Design**
   ```dart
   // Use MediaQuery for responsive sizing
   height: MediaQuery.of(context).size.height * 0.1,
   width: MediaQuery.of(context).size.width * 0.8,
   ```

3. **Color Usage**
   ```dart
   // Define colors as constants
   class AppColors {
     static const Color primaryPurple = Color(0xFF0D0274);
     static const Color accentOrange = Color(0xFFFE793E);
   }
   ```

## Development Workflow

### Git Workflow

1. **Branch Naming**
   ```bash
   feature/add-shopping-cart
   bugfix/fix-image-loading
   hotfix/critical-crash-fix
   ```

2. **Commit Messages**
   ```bash
   feat: add shopping cart functionality
   fix: resolve image loading issue on Android
   docs: update API documentation
   style: improve code formatting
   refactor: extract reusable components
   test: add widget tests for home screen
   ```

3. **Pull Request Process**
   - Create feature branch
   - Make changes with tests
   - Update documentation
   - Submit PR with description
   - Code review and merge

### Development Process

1. **Feature Development**
   ```bash
   # Create feature branch
   git checkout -b feature/new-feature
   
   # Make changes
   flutter run  # Test continuously
   
   # Run tests
   flutter test
   
   # Format code
   flutter format .
   
   # Analyze code
   flutter analyze
   
   # Commit changes
   git add .
   git commit -m "feat: add new feature"
   ```

2. **Code Quality Checks**
   ```bash
   # Run all quality checks
   flutter analyze
   flutter test
   flutter format --set-exit-if-changed .
   ```

## Testing Guidelines

### Unit Testing

```dart
// test/models/food_item_test.dart
import 'package:flutter_test/flutter_test.dart';
import 'package:food/models/food_item.dart';

void main() {
  group('FoodItem', () {
    test('should create food item with correct properties', () {
      final foodItem = FoodItem(
        name: 'Pizza',
        price: 15.99,
        image: 'assets/pizza.png',
      );
      
      expect(foodItem.name, 'Pizza');
      expect(foodItem.price, 15.99);
    });
  });
}
```

### Widget Testing

```dart
// test/screens/home_screen_test.dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:food/screens/home.dart';

void main() {
  testWidgets('HomeScreen displays favourite dishes title', (tester) async {
    await tester.pumpWidget(
      MaterialApp(home: HomeScreen()),
    );
    
    expect(find.text('Favourite Dishes'), findsOneWidget);
    expect(find.text('Popular Dishes'), findsOneWidget);
  });
}
```

### Integration Testing

```dart
// integration_test/app_test.dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:integration_test/integration_test.dart';
import 'package:food/main.dart' as app;

void main() {
  IntegrationTestWidgetsFlutterBinding.ensureInitialized();
  
  testWidgets('app launches and displays home screen', (tester) async {
    app.main();
    await tester.pumpAndSettle();
    
    expect(find.text('Favourite Dishes'), findsOneWidget);
  });
}
```

### Testing Commands

```bash
# Run all tests
flutter test

# Run specific test file
flutter test test/screens/home_screen_test.dart

# Run tests with coverage
flutter test --coverage

# Run integration tests
flutter test integration_test/
```

## Performance Optimization

### Image Optimization

1. **Asset Images**
   ```dart
   // Use appropriate image sizes
   Image.asset(
     'assets/food.png',
     fit: BoxFit.cover,
     width: 100,
     height: 100,
   )
   ```

2. **Caching**
   ```dart
   // Use cached network images for future API integration
   CachedNetworkImage(
     imageUrl: 'https://api.example.com/food.jpg',
     placeholder: (context, url) => CircularProgressIndicator(),
   )
   ```

### Widget Performance

1. **ListView Optimization**
   ```dart
   ListView.builder(
     itemCount: items.length,
     itemBuilder: (context, index) => FoodCard(items[index]),
   )
   ```

2. **Avoid Rebuilds**
   ```dart
   // Use const constructors when possible
   const Text('Favourite Dishes')
   
   // Extract widgets that don't change
   static const Widget titleWidget = Text('App Title');
   ```

### Memory Management

```dart
// Dispose controllers and streams
class ExampleScreen extends StatefulWidget {
  @override
  _ExampleScreenState createState() => _ExampleScreenState();
}

class _ExampleScreenState extends State<ExampleScreen> {
  ScrollController _scrollController = ScrollController();
  
  @override
  void dispose() {
    _scrollController.dispose();
    super.dispose();
  }
}
```

## Debugging

### Debug Tools

1. **Flutter Inspector**
   ```bash
   # Launch with debug mode
   flutter run --debug
   
   # Open Flutter Inspector in IDE
   ```

2. **Debug Console**
   ```dart
   // Add debug prints
   debugPrint('Current user: $userName');
   
   // Assert conditions
   assert(foodItems.isNotEmpty, 'Food items cannot be empty');
   ```

3. **Performance Profiling**
   ```bash
   flutter run --profile
   flutter run --trace-startup
   ```

### Common Debug Scenarios

1. **Layout Issues**
   ```dart
   // Add colored containers to visualize layout
   Container(
     color: Colors.red.withOpacity(0.3),
     child: YourWidget(),
   )
   ```

2. **State Issues**
   ```dart
   // Log state changes
   setState(() {
     debugPrint('State changing: $oldValue -> $newValue');
     value = newValue;
   });
   ```

## Deployment

### Android Deployment

1. **Debug Build**
   ```bash
   flutter build apk --debug
   ```

2. **Release Build**
   ```bash
   # Generate keystore
   keytool -genkey -v -keystore android/app/key.jks
   
   # Build release APK
   flutter build apk --release
   
   # Build App Bundle (recommended)
   flutter build appbundle --release
   ```

### iOS Deployment

```bash
# Build iOS (macOS only)
flutter build ios --release

# Archive and upload via Xcode
open ios/Runner.xcworkspace
```

### Web Deployment

```bash
# Build for web
flutter build web

# Serve locally
flutter run -d chrome
```

## Common Issues

### Build Issues

1. **Dependency Conflicts**
   ```bash
   flutter clean
   flutter pub get
   flutter pub deps
   ```

2. **Platform Issues**
   ```bash
   # Android
   flutter doctor --android-licenses
   
   # iOS
   cd ios && pod install
   ```

### Runtime Issues

1. **Image Loading**
   ```dart
   // Check asset path in pubspec.yaml
   flutter:
     assets:
       - assets/images/
   ```

2. **Performance Issues**
   ```bash
   # Profile performance
   flutter run --profile
   flutter run --trace-startup
   ```

### Development Tips

1. **Hot Reload**
   - Use `r` for hot reload
   - Use `R` for hot restart
   - Use `q` to quit

2. **State Management**
   - Consider Provider or Riverpod for complex state
   - Use StatefulWidget for local state
   - Minimize widget rebuilds

3. **Code Organization**
   - Extract reusable widgets
   - Use meaningful file names
   - Group related functionality

## Next Steps

For expanding the application:

1. **State Management**: Implement Provider or BLoC
2. **Navigation**: Add proper routing with Navigator 2.0
3. **API Integration**: Connect to backend services
4. **Local Storage**: Add SharedPreferences or Hive
5. **Authentication**: Implement user login/signup
6. **Testing**: Expand test coverage
7. **CI/CD**: Set up automated testing and deployment