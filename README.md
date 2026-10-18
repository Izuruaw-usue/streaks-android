# Streaks — habit tracker (Lab 2)

Android app (Kotlin, MVVM, Room) implementing the Figma design:
https://www.figma.com/design/2nH2vz6hiFV0LiKBJKQfIf

## What it does
Sign up / log in, create daily or weekly habits (icon, frequency, reminder time), check them off every day,
see streaks, a monthly calendar and statistics. Everything is stored locally and survives app restarts.

## Screens (Figma frame → Android)
| Figma frame | Android |
|---|---|
| 01 Splash | `SplashFragment` (routes to onboarding / login / home) |
| 02 Onboarding | `OnboardingFragment` (ViewPager2, 3 pages) |
| 03 Sign up – error, 04 Sign up | `SignUpFragment` (error state shown by validation) |
| 05 Log in | `LoginFragment` |
| 06 Home – empty, 08 Home | `HomeFragment` (empty state + list + progress card + FAB) |
| 07 Add habit, 10 Edit habit | `HabitFormFragment` (same fragment, `habitId` argument) |
| 09 Habit details | `HabitDetailsFragment` |
| 11 Calendar | `CalendarFragment` |
| 12 Statistics | `StatsFragment` |
| 13 Profile | `ProfileFragment` |
| 14 Settings | `SettingsFragment` |

14 designed screens → 11 fragments (single-activity, Navigation Component, `BottomNavigationView` for
Today / Calendar / Stats / Profile).

## Architecture
```
data/   Room (Habit, Completion, HabitDao, AppDatabase), UserPrefs (SharedPreferences), HabitViewModel
ui/     one package per screen (Fragment + ViewBinding)
util/   small helpers (collectFlow, time picker, icon map)
```
- **MVVM**: `HabitViewModel` exposes one `StateFlow<Snapshot>` (habits + per-day counts); fragments collect it with
  `repeatOnLifecycle`.
- **Persistence**: Room for habits/completions, SharedPreferences for session, profile and settings.
- **Theming**: colors/typography taken from the Figma styles (`colors.xml`, `styles.xml`); dark mode via `values-night`.

## How to run
1. Install **Android Studio** (Koala or newer) with Android SDK 34.
2. `File → Open` → select the `Streaks` folder. Wait for Gradle sync.
3. Pick an emulator (API 26+) or a USB device and press **Run ▶**.

### Build the APK
`Build → Build Bundle(s) / APK(s) → Build APK(s)`
or in a terminal: `./gradlew assembleDebug`
→ `app/build/outputs/apk/debug/app-debug.apk` (install with `adb install app-debug.apk`).

## How to use
1. Swipe through onboarding (or Skip) → **Sign up** (name, valid email, password ≥ 6 chars).
2. On **Today** tap **Add habit** / **+**, enter a name, choose an icon and frequency, **Save habit**.
3. Tap the circle on a habit to check it off; tap the card to see streak, best streak and last 7 days.
4. **Calendar** and **Stats** update automatically. **Profile → Settings** has dark mode and reminder time.
5. Close and reopen the app — data and login are kept. **Profile → Log out** returns to the login screen.

## Known limitations
- Reminder time and the reminders switch are saved, but no system notifications are scheduled yet.
- Accounts are local to the device (demo only; password stored as a SHA-256 hash).
