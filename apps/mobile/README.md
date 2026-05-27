# Mobile (frontend)

Purpose
- Mobile client for the product. Choose an implementation (React Native / Flutter / native).

Local run (placeholder - adapt to chosen stack):

React Native example:
```bash
cd apps/mobile
npm ci
npx react-native start
# run on iOS / Android as required
```

Flutter example:
```bash
cd apps/mobile
flutter pub get
flutter run
```

Notes
- Decide on a single mobile strategy and document it here.
- Reuse shared types from `../../packages/shared` where possible.
