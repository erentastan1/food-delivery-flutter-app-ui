# Food Delivery App 🍕📱

A beautiful and modern food delivery Flutter application with an elegant UI design inspired by contemporary food delivery services.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Screenshots](#screenshots)
- [Installation](#installation)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Dependencies](#dependencies)
- [Building the App](#building-the-app)
- [Testing](#testing)
- [Contributing](#contributing)
- [Design Credits](#design-credits)
- [License](#license)

## 🎯 Overview

This Flutter application showcases a modern food delivery app interface with:
- Gradient-based dark theme UI
- Beautiful card layouts for food items
- Horizontal scrolling for popular dishes
- Star rating system
- Price display with custom styling
- Responsive design for both Android and iOS

## ✨ Features

### 🏠 Home Screen
- **Date Display**: Shows current date at the top
- **Favourite Dishes Section**: Featured food items with images, descriptions, and prices
- **Popular Dishes Section**: Horizontally scrollable list of trending food items
- **Star Rating System**: 5-star rating display for each dish
- **Price Display**: Formatted price in KES (Kenyan Shillings)

### 🎨 UI/UX Features
- **Gradient Backgrounds**: Beautiful purple/blue gradient theme throughout the app
- **Card-based Design**: Modern card layout with shadows and rounded corners
- **Responsive Layout**: Adaptive design that works on different screen sizes
- **Custom Icons**: Heart icon for favorites, menu icon for navigation
- **Smooth Scrolling**: Single child scroll view with scrollbar

### 🍝 Food Items Featured
- **Kachi kocha Pitha**: Special crunch flavours (KES 750)
- **Italian Noodles**: Popular dish with 5-star rating (KES 100)
- **Food Images**: High-quality food photography assets

## 📱 Screenshots

![App Screenshot](screenshot/1.png)

*The main home screen showcasing the gradient design and food cards*

## 🚀 Installation

### Prerequisites
- [Flutter SDK](https://flutter.dev/docs/get-started/install) (>=2.1.0 <3.0.0)
- [Dart SDK](https://dart.dev/get-dart)
- Android Studio / VS Code with Flutter extensions
- Android SDK / Xcode (for iOS development)

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd food-delivery-app
   ```

2. **Install dependencies**
   ```bash
   flutter pub get
   ```

3. **Run the application**
   ```bash
   flutter run
   ```

## 🏗️ Getting Started

### For New Flutter Developers

If this is your first Flutter project, here are some helpful resources:

- 📚 [Lab: Write your first Flutter app](https://flutter.dev/docs/get-started/codelab)
- 🍳 [Cookbook: Useful Flutter samples](https://flutter.dev/docs/cookbook)
- 📖 [Flutter Documentation](https://flutter.dev/docs) - Tutorials, samples, and API reference

### Development Setup

1. **Check Flutter installation**
   ```bash
   flutter doctor
   ```

2. **Create a new emulator/simulator** (if needed)
   ```bash
   flutter emulators --create
   ```

3. **Run on specific platform**
   ```bash
   # For Android
   flutter run -d android
   
   # For iOS (macOS only)
   flutter run -d ios
   ```

## 📁 Project Structure

```
food/
├── android/                 # Android-specific files
├── assets/                  # Image and other assets
│   ├── food.png            # Food item image
│   └── spangetti.png       # Spaghetti image
├── ios/                    # iOS-specific files
├── lib/                    # Main Dart code
│   ├── main.dart          # App entry point
│   └── screens/           # Screen widgets
│       └── home.dart      # Home screen implementation
├── screenshot/             # App screenshots
│   └── 1.png              # Main screenshot
├── test/                   # Test files
│   └── widget_test.dart   # Widget tests
├── .gitignore             # Git ignore rules
├── .metadata              # Flutter metadata
├── pubspec.yaml           # Project dependencies and configuration
├── pubspec.lock           # Locked dependency versions
└── README.md              # This file
```

### Key Files Explained

- **`lib/main.dart`**: Application entry point, sets up MaterialApp with indigo theme
- **`lib/screens/home.dart`**: Main home screen with all UI components
- **`pubspec.yaml`**: Project configuration, dependencies, and assets
- **`assets/`**: Contains food images used in the app

## 📦 Dependencies

### Main Dependencies
- **`flutter`**: Flutter SDK
- **`cupertino_icons ^0.1.2`**: iOS-style icons

### Dev Dependencies
- **`flutter_test`**: Flutter testing framework

### Assets
- **Material Design Icons**: Enabled for consistent iconography
- **Custom Food Images**: 
  - `assets/spangetti.png`: Spaghetti dish image
  - `assets/food.png`: General food item image

## 🔨 Building the App

### Debug Build
```bash
# Run in debug mode
flutter run

# Build debug APK
flutter build apk --debug
```

### Release Build
```bash
# Build release APK
flutter build apk --release

# Build iOS (macOS only)
flutter build ios --release
```

### Build for Web
```bash
flutter build web
```

## 🧪 Testing

### Running Tests
```bash
# Run all tests
flutter test

# Run specific test file
flutter test test/widget_test.dart

# Run tests with coverage
flutter test --coverage
```

### Test Structure
- **Widget Tests**: Basic widget testing setup in `test/widget_test.dart`
- **Integration Tests**: Can be added in `integration_test/` directory
- **Unit Tests**: Can be added for business logic testing

### Adding New Tests
1. Create test files in the `test/` directory
2. Import necessary testing libraries
3. Write test cases using the `testWidgets` function
4. Run tests to ensure functionality

## 🎨 Customization

### Color Scheme
The app uses a gradient color scheme with these primary colors:
- Primary: `#0D0274` (Deep Purple)
- Secondary variations: `#0D0272`, `#0D016B`, `#0D0265`
- Accent: `#FE793E` (Orange for prices)
- Text: `#9891CB` (Light purple for secondary text)

### Modifying UI Components

#### Changing Colors
```dart
// In home.dart, modify gradient colors
colors: [
  Color(0xFF0D0274),  // Change these hex values
  Color(0xFF0D0272),
  // ... more colors
]
```

#### Adding New Food Items
1. Add new images to `assets/` folder
2. Update `pubspec.yaml` to include new assets
3. Modify the `_buildCard()` or `_buildDish()` functions in `home.dart`

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Make your changes**
4. **Add tests** for new functionality
5. **Commit your changes**
   ```bash
   git commit -m 'Add some amazing feature'
   ```
6. **Push to the branch**
   ```bash
   git push origin feature/amazing-feature
   ```
7. **Open a Pull Request**

### Development Guidelines
- Follow [Dart style guidelines](https://dart.dev/guides/language/effective-dart/style)
- Add comments for complex logic
- Write tests for new features
- Update documentation as needed

## 🎨 Design Credits

Special thanks to [Moinul Ahsan](https://dribbble.com/moin40) for the appealing food delivery user interface [mock-up](https://dribbble.com/shots/6846120-Food-Delivery-App-Concept) that inspired this implementation.

## 📄 License

This project is available under the MIT License. See the LICENSE file for more details.

## 🆘 Support

If you encounter any issues or have questions:

1. **Check the [Flutter documentation](https://flutter.dev/docs)**
2. **Search existing issues** in the repository
3. **Create a new issue** with detailed information about your problem
4. **Join the Flutter community** on [Discord](https://discord.gg/flutter) or [Stack Overflow](https://stackoverflow.com/questions/tagged/flutter)

## 🚧 Future Enhancements

Potential features for future development:
- 🛒 Shopping cart functionality
- 👤 User authentication and profiles
- 🔍 Search and filtering capabilities
- ⭐ User reviews and ratings
- 🚚 Order tracking
- 💳 Payment integration
- 📍 Location services and delivery tracking
- 🔔 Push notifications
- 🌙 Light/dark theme toggle

---

**Happy Coding! 🎉**

*Made with ❤️ using Flutter*
