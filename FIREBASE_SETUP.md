# Firebase AI Setup Guide

This guide will help you set up Firebase AI in your LMHub application.

## Prerequisites

1. A Firebase project
2. Flutter SDK installed
3. Firebase CLI (optional, for advanced setup)

## Step 1: Create a Firebase Project

1. Go to the [Firebase Console](https://console.firebase.google.com/)
2. Click "Add project"
3. Follow the wizard to create your project

## Step 2: Enable AI/Generative AI Features

1. In the Firebase Console, go to "Build" > "AI/Generative AI"
2. Enable the features you need (Gemini API, etc.)
3. Create and copy your API key

## Step 3: Add Firebase Configuration Files

### For Android (google-services.json):

1. Go to Project Settings in Firebase Console
2. Add an Android app with your package name (e.g., `com.example.lmhub`)
3. Download `google-services.json`
4. Place it in `android/app/` directory

### For iOS (GoogleService-Info.plist):

1. In Firebase Console, add an iOS app
2. Download `GoogleService-Info.plist`
3. Add it to your Xcode project (in ios/Runner directory)
4. In Xcode, select the file and ensure "Runner" target is selected
5. Check "Copy items if needed" and click "Add"

## Step 4: Configure Firebase Options (Optional)

If you have multiple environments or configurations, generate Firebase options:

1. Install Firebase CLI: `npm install -g firebase-tools`
2. Login: `firebase login`
3. Initialize: `firebase init` in your project root
4. Select "Flutter" and follow the prompts
5. This will generate `lib/firebase_options.dart`

### Manual Configuration:

Create `lib/firebase_options.dart` (optional, if you want to manually configure):

```dart
import 'package:firebase_core/firebase_core.dart' show FirebaseOptions;
import 'package:flutter/foundation.dart'
    show defaultTargetPlatform, kIsWeb, TargetPlatform;

class DefaultFirebaseOptions {
  static FirebaseOptions get currentPlatform {
    if (kIsWeb) {
      return web;
    }
    switch (defaultTargetPlatform) {
      case TargetPlatform.android:
        return android;
      case TargetPlatform.iOS:
        return ios;
      case TargetPlatform.macOS:
        return macos;
      default:
        throw UnsupportedError(
          'DefaultFirebaseOptions have not been configured for this platform.',
        );
    }
  }

  static const FirebaseOptions web = FirebaseOptions(
    apiKey: 'YOUR_API_KEY',
    appId: 'YOUR_APP_ID',
    messagingSenderId: 'YOUR_SENDER_ID',
    projectId: 'YOUR_PROJECT_ID',
  );

  static const FirebaseOptions android = FirebaseOptions(
    apiKey: 'YOUR_API_KEY',
    appId: 'YOUR_APP_ID',
    messagingSenderId: 'YOUR_SENDER_ID',
    projectId: 'YOUR_PROJECT_ID',
  );

  static const FirebaseOptions ios = FirebaseOptions(
    apiKey: 'YOUR_API_KEY',
    appId: 'YOUR_APP_ID',
    messagingSenderId: 'YOUR_SENDER_ID',
    projectId: 'YOUR_PROJECT_ID',
    iosClientId: 'YOUR_IOS_CLIENT_ID',
  );

  static const FirebaseOptions macos = FirebaseOptions(
    apiKey: 'YOUR_API_KEY',
    appId: 'YOUR_APP_ID',
    messagingSenderId: 'YOUR_SENDER_ID',
    projectId: 'YOUR_PROJECT_ID',
    iosClientId: 'YOUR_IOS_CLIENT_ID',
  );
}
```

Then update `lib/main.dart`:

```dart
import 'package:firebase_core/firebase_core.dart';
import 'firebase_options.dart';

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await EasyLocalization.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );
  // ...
}
```

## Step 5: Add Firebase AI Provider

1. Open the app
2. Go to Settings > Providers
3. Tap "Add Provider"
4. Select "FIREBASEAI" from the dropdown
5. Enter your Firebase API Key
6. Optionally fetch and add models
7. Save

## Step 6: Test the Integration

1. Create a new chat or use an existing one
2. Send a message
3. The app should now use Firebase AI to generate responses

## Troubleshooting

### "Firebase not initialized" error
- Ensure you've added the configuration files (google-services.json or GoogleService-Info.plist)
- Check that Firebase is initialized in main.dart

### "Invalid API Key" error
- Verify your API key is correct
- Ensure the key has the necessary permissions in Firebase Console

### Build errors
- Clean and rebuild: `flutter clean && flutter pub get`
- Ensure platform-specific files are properly added

### Models not showing up
- Ensure you have at least one model added to your provider
- Try fetching models from the provider configuration screen

## Additional Resources

- [Firebase Flutter Documentation](https://firebase.google.com/docs/flutter/setup)
- [Firebase Generative AI Documentation](https://firebase.google.com/docs/ai)
- [Gemini API Documentation](https://ai.google.dev/)

## Support

If you encounter issues:
1. Check the Firebase Console for errors
2. Review the Flutter console logs for detailed error messages
3. Consult the official Firebase and Flutter documentation