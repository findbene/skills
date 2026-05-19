# Extended Details

## Examples

**Example 1 — Convert Stitch mobile design to Expo app**
- Input: "Build the iOS + Android version of this Stitch dashboard with Expo"
- Action: Fetch screens → produce `app/(tabs)/index.tsx`, `app/(tabs)/profile.tsx`, shared `theme/colors.ts` → tab navigator via Expo Router → useColorScheme dark mode
- Output: Expo project with 5 screens, dark mode verified, runs in iOS sim and Android emulator

**Example 2 — Single screen with form**
- Input: "Just the signup screen as a React Native component"
- Action: Map form layout to `View` + `TextInput` + `Pressable` → 48dp touch targets → keyboard avoiding view → accessibility labels
- Output: `SignupScreen.tsx`, typed props, validated form, keyboard-safe, dark mode supported
