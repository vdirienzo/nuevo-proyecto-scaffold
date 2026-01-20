# {{PROJECT_NAME}} - Flutter Mobile App

A modern, production-ready Flutter mobile application built with clean architecture and best practices.

## Author

{{AUTHOR}}

## Features

- 🎨 **Material 3 Design** - Modern UI with dynamic theming
- 🌓 **Dark/Light Mode** - Automatic theme switching
- 🔐 **JWT Authentication** - Secure token-based auth
- 🏗️ **Clean Architecture** - Separation of concerns (data/domain/presentation)
- 🔄 **State Management** - Riverpod 2.0 for reactive state
- 🧭 **Type-safe Navigation** - GoRouter with deep linking
- 🌐 **Internationalization** - Multi-language support (en, es)
- 📝 **Form Validation** - Comprehensive input validators
- 🎯 **Error Handling** - Centralized exception handling
- 🔒 **Secure Storage** - flutter_secure_storage for tokens
- 📡 **API Client** - Dio with interceptors and retry logic
- 🧪 **Testing** - Unit and widget tests with Mockito
- 📦 **Code Generation** - Freezed, json_serializable, Riverpod Generator

## Tech Stack

| Category | Package | Version |
|----------|---------|---------|
| **Framework** | Flutter | 3.x |
| **State Management** | Riverpod | ^2.4.9 |
| **Navigation** | GoRouter | ^13.0.0 |
| **HTTP Client** | Dio | ^5.4.0 |
| **Storage** | flutter_secure_storage | ^9.0.0 |
| **Code Generation** | Freezed, json_serializable | Latest |
| **Testing** | Mockito, flutter_test | Latest |

## Project Structure

```
lib/
├── app.dart                    # App root widget
├── main.dart                   # Entry point
├── core/                       # Core infrastructure
│   ├── constants/              # App constants
│   ├── theme/                  # Material 3 theme
│   ├── router/                 # GoRouter configuration
│   ├── network/                # API client & interceptors
│   └── utils/                  # Validators & extensions
├── features/                   # Feature modules (Clean Architecture)
│   ├── auth/
│   │   ├── data/               # Repositories & local storage
│   │   ├── domain/             # Models & entities
│   │   ├── presentation/       # Screens & widgets
│   │   └── providers/          # Riverpod providers
│   ├── home/
│   └── profile/
├── shared/                     # Shared widgets & providers
│   ├── widgets/                # Reusable UI components
│   └── providers/              # Global providers
└── l10n/                       # Localization files
```

## Getting Started

### Prerequisites

- Flutter SDK 3.x or higher
- Dart SDK 3.0 or higher
- Android Studio / Xcode (for mobile development)
- VS Code (recommended)

### Installation

1. **Clone the repository**
```bash
git clone <repository-url>
cd {{PROJECT_NAME}}
```

2. **Install dependencies**
```bash
flutter pub get
```

3. **Generate code**
```bash
flutter pub run build_runner build --delete-conflicting-outputs
```

4. **Configure environment**
```bash
cp .env.example .env
# Edit .env with your API endpoint
```

5. **Run the app**
```bash
flutter run
```

## Development

### Code Generation

Run code generation for Freezed, JSON serialization, and Riverpod:

```bash
# One-time generation
flutter pub run build_runner build --delete-conflicting-outputs

# Watch mode (auto-generate on changes)
flutter pub run build_runner watch --delete-conflicting-outputs
```

### Testing

```bash
# Run all tests
flutter test

# Run with coverage
flutter test --coverage

# Run specific test file
flutter test test/unit/auth_test.dart
```

### Linting

```bash
# Analyze code
flutter analyze

# Format code
dart format .
```

## Build

### Android

```bash
# Debug APK
flutter build apk --debug

# Release APK
flutter build apk --release

# App Bundle (for Play Store)
flutter build appbundle --release
```

### iOS

```bash
# Debug build
flutter build ios --debug

# Release build
flutter build ios --release
```

### Web

```bash
flutter build web --release
```

## Configuration

### API Endpoint

Edit `.env` file:

```env
API_BASE_URL=https://your-api-endpoint.com/api
API_TIMEOUT=30000
ENABLE_LOGGING=true
```

### Theme

Customize theme in `lib/core/theme/`:
- `app_colors.dart` - Color scheme
- `app_typography.dart` - Text styles
- `app_theme.dart` - Theme configuration

### Routing

Add new routes in `lib/core/router/`:
- `routes.dart` - Route constants
- `app_router.dart` - GoRouter configuration

## Architecture

### Clean Architecture Layers

1. **Domain Layer** (`domain/`)
   - Models and entities
   - Business logic interfaces
   - Freezed data classes

2. **Data Layer** (`data/`)
   - Repository implementations
   - API clients
   - Local storage

3. **Presentation Layer** (`presentation/`)
   - Screens and widgets
   - UI logic
   - Riverpod state management

### State Management

Using **Riverpod** with `StateNotifier`:

```dart
// Provider definition
final authProvider = StateNotifierProvider<AuthNotifier, AuthState>((ref) {
  return AuthNotifier(ref.watch(authRepositoryProvider));
});

// Usage in widget
class MyWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final authState = ref.watch(authProvider);
    // Use state...
  }
}
```

## Localization

Add translations in `lib/l10n/`:
- `app_en.arb` - English
- `app_es.arb` - Spanish

Generate localizations:
```bash
flutter gen-l10n
```

Usage:
```dart
import 'package:flutter_gen/gen_l10n/app_localizations.dart';

Text(AppLocalizations.of(context)!.welcomeBack)
```

## Docker

Build and run with Docker:

```bash
# Build image
docker build -t {{PROJECT_NAME}} .

# Run container
docker run -p 8080:80 {{PROJECT_NAME}}
```

Access at `http://localhost:8080`

## CI/CD

The project includes GitHub Actions workflows for:
- ✅ Linting and analysis
- ✅ Testing with coverage
- ✅ Building APK/iOS
- ✅ Web deployment

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Code Quality

- Follow [Effective Dart](https://dart.dev/guides/language/effective-dart) guidelines
- Use `analysis_options.yaml` linting rules
- Write tests for new features
- Maintain code coverage above 80%

## Common Issues

### Build Runner Conflicts

```bash
flutter pub run build_runner build --delete-conflicting-outputs
```

### Clear Cache

```bash
flutter clean
flutter pub get
```

### iOS CocoaPods Issues

```bash
cd ios
pod deintegrate
pod install
```

## License

[Your License Here]

## Support

For issues and questions:
- Create an issue on GitHub
- Email: [your-email@example.com]

## Changelog

### [1.0.0] - 2024-XX-XX

#### Added
- Initial release
- JWT authentication
- Material 3 theming
- Clean architecture implementation
- Riverpod state management
- GoRouter navigation
- Internationalization (en, es)
- Unit and widget tests
- Docker support

---

**Built with ❤️ using Flutter**
