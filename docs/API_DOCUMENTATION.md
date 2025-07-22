# API Documentation

## Overview

This document provides detailed API documentation for the Food Delivery App's components and widgets.

## Table of Contents

- [Main App](#main-app)
- [Home Screen](#home-screen)
- [Widget Components](#widget-components)
- [Styling and Theming](#styling-and-theming)
- [Assets](#assets)

## Main App

### MyApp Class

**File**: `lib/main.dart`

```dart
class MyApp extends StatelessWidget
```

**Description**: Root application widget that sets up the MaterialApp configuration.

**Properties**:
- `title`: "Flutter Demo"
- `theme`: MaterialApp theme with indigo primary swatch
- `routes`: Application routing configuration

**Routes**:
- `HomeScreen.routeName`: Routes to the HomeScreen widget

## Home Screen

### HomeScreen Class

**File**: `lib/screens/home.dart`

```dart
class HomeScreen extends StatelessWidget
```

**Properties**:
- `routeName`: Static route name "/" for navigation

**UI Components**:
- AppBar with gradient background
- Scrollable body with gradient background
- Header section with date and title
- Favourite dishes cards
- Popular dishes horizontal list

### Color Scheme

**Primary Gradient Colors**:
- `#0D0274` - Primary dark purple
- `#0D0272` - Secondary purple
- `#0D016B` - Mid purple
- `#0D0265` - Light purple
- `#0E0466` - Variant purple
- `#12085A` - Accent purple

**Accent Colors**:
- `#FE793E` - Orange (for prices)
- `#9891CB` - Light purple (for secondary text)
- `#FFBF32` - Yellow (for stars)

## Widget Components

### _buildHeader Function

**Signature**: `Widget _buildHeader(BuildContext context)`

**Description**: Builds the main header section containing date, title, and food cards.

**Returns**: Container with column layout containing:
- Date text
- "Favourite Dishes" title
- Two favourite dish cards
- Popular dishes section

### _buildCard Function

**Signature**: `Widget _buildCard(BuildContext context)`

**Description**: Creates a card widget for favourite food items.

**Features**:
- Gradient background with rounded corners
- Shadow effects
- Food image display
- Text information (name, description, price)
- Responsive sizing

**Card Content**:
- Food name: "Kachi kocha Pitha"
- Description: "Special Crunch flavours"
- Price: "KES 750"
- Image: `assets/food.png`

### _buildPopular Function

**Signature**: `Widget _buildPopular(BuildContext context)`

**Description**: Creates the popular dishes section with horizontal scrolling.

**Features**:
- Section title with "View All" option
- Horizontal ListView with 4 items
- Fixed height container (300px)
- Scrollable dish cards

### _buildDish Function

**Signature**: `Widget _buildDish(BuildContext context)`

**Description**: Creates individual dish cards for the popular section.

**Features**:
- Stack layout with overlapping image
- Gradient card background
- Star rating display
- Price information
- Fixed dimensions (150x250)

**Dish Content**:
- Dish name: "Italian Noodles"
- Rating: 5 stars
- Price: "KES 100"
- Image: `assets/spangetti.png`

### _buildStars Function

**Signature**: `Widget _buildStars(BuildContext context)`

**Description**: Creates a 5-star rating display widget.

**Returns**: Row of 5 star icons with golden color (`#FFBF32`)

## Styling and Theming

### Text Styles

**Date Text**:
```dart
TextStyle(
  color: Color(0xFF9891CB),
  fontWeight: FontWeight.w600,
  fontSize: 18
)
```

**Title Text**:
```dart
TextStyle(
  color: Colors.white,
  fontWeight: FontWeight.bold,
  fontSize: 34
)
```

**Food Name**:
```dart
TextStyle(
  color: Colors.white,
  fontWeight: FontWeight.bold,
  fontSize: 18
)
```

**Description Text**:
```dart
TextStyle(
  color: Color(0xFF9891CB),
  fontWeight: FontWeight.w600,
  fontSize: 14
)
```

**Price Text**:
```dart
TextStyle(
  color: Color(0xFFFE793E),
  fontSize: 18,
  fontWeight: FontWeight.bold
)
```

### Container Styling

**Card Decoration**:
```dart
BoxDecoration(
  borderRadius: BorderRadius.circular(20),
  boxShadow: [
    BoxShadow(
      color: Colors.black12,
      offset: Offset(4, 3),
      blurRadius: 5.0,
      spreadRadius: 2.3
    )
  ],
  gradient: LinearGradient(...)
)
```

### Responsive Sizing

**Image Dimensions**:
- Card images: `height: MediaQuery.of(context).size.height / 9`
- Card images: `width: MediaQuery.of(context).size.width / 4`
- Dish images: `height: MediaQuery.of(context).size.height / 8`
- Dish images: `width: MediaQuery.of(context).size.width / 2`

## Assets

### Image Assets

**food.png**:
- Used in favourite dishes cards
- Location: `assets/food.png`
- Format: PNG
- Usage: Featured food item display

**spangetti.png**:
- Used in popular dishes section
- Location: `assets/spangetti.png`
- Format: PNG
- Usage: Italian noodles display

### Asset Configuration

Assets are configured in `pubspec.yaml`:
```yaml
flutter:
  assets:
   - assets/spangetti.png
   - assets/food.png
```

## Navigation

### Route Configuration

Routes are defined in `main.dart`:
```dart
routes: {
  HomeScreen.routeName: (context) {
    return HomeScreen();
  }
}
```

**Available Routes**:
- `/`: HomeScreen (default route)

## Performance Considerations

### ListView Optimization
- Uses `ListView.builder` for efficient scrolling
- Fixed `itemCount: 4` for popular dishes
- Horizontal scrolling direction for better UX

### Image Loading
- Uses `Image.asset` for local image loading
- Images are preloaded during app initialization
- Responsive sizing based on screen dimensions

## Testing API

### Widget Testing
Basic widget test structure in `test/widget_test.dart`:
```dart
testWidgets('Counter increments smoke test', (WidgetTester tester) async {
  await tester.pumpWidget(MyApp());
  // Test implementation
});
```

**Available Test Methods**:
- `tester.pumpWidget()`: Render widget
- `find.text()`: Find text widgets
- `find.byIcon()`: Find icon widgets
- `tester.tap()`: Simulate tap gestures
- `expect()`: Assert test conditions

## Error Handling

Currently, the app implements basic error handling through Flutter's built-in mechanisms:
- Widget build error boundaries
- Asset loading fallbacks
- Route navigation error handling

## Accessibility

The app follows basic accessibility guidelines:
- Semantic text labels
- Proper contrast ratios
- Touch target sizing
- Screen reader compatibility

## Future API Extensions

Potential API expansions:
- User authentication endpoints
- Food item data models
- Cart management functions
- Order processing workflows
- Payment integration APIs
- Location services integration