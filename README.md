# Streaks — трекер привычек (Лабораторная работа №2)

Android-приложение (Kotlin, MVVM, Room), реализующее дизайн из Figma:
https://www.figma.com/design/2nH2vz6hiFV0LiKBJKQfIf

## Что делает приложение
Регистрация и вход, создание ежедневных и еженедельных привычек (иконка, периодичность, время напоминания),
ежедневная отметка выполнения, серии дней подряд (streak), календарь на месяц и статистика.
Все данные хранятся локально и сохраняются после перезапуска приложения.

## Экраны (фрейм Figma → Android)
| Фрейм Figma | Android |
|---|---|
| 01 Splash | `SplashFragment` (направляет на onboarding / login / home) |
| 02 Onboarding | `OnboardingFragment` (ViewPager2, 3 страницы) |
| 03 Sign up – error, 04 Sign up | `SignUpFragment` (состояние ошибки показывается при валидации) |
| 05 Log in | `LoginFragment` |
| 06 Home – empty, 08 Home | `HomeFragment` (пустое состояние + список + progress card + FAB) |
| 07 Add habit, 10 Edit habit | `HabitFormFragment` (один фрагмент, аргумент `habitId`) |
| 09 Habit details | `HabitDetailsFragment` |
| 11 Calendar | `CalendarFragment` |
| 12 Statistics | `StatsFragment` |
| 13 Profile | `ProfileFragment` |
| 14 Settings | `SettingsFragment` |

14 экранов в дизайне → 11 фрагментов (одна Activity, Navigation Component, `BottomNavigationView` для
Today / Calendar / Stats / Profile).

## Архитектура
```
data/   Room (Habit, Completion, HabitDao, AppDatabase), UserPrefs (SharedPreferences), HabitViewModel
ui/     по одному пакету на экран (Fragment + ViewBinding)
util/   вспомогательные функции (collectFlow, выбор времени, карта иконок)
```
- **MVVM**: `HabitViewModel` отдаёт один `StateFlow<Snapshot>` (привычки + количество отметок по дням);
  фрагменты подписываются на него через `repeatOnLifecycle`.
- **Хранение данных**: Room для привычек и отметок, SharedPreferences для сессии, профиля и настроек.
- **Тема оформления**: цвета и типографика взяты из стилей Figma (`colors.xml`, `styles.xml`);
  тёмная тема через `values-night`.

## Как запустить
1. Установите **Android Studio** (Koala или новее) с Android SDK 34.
2. `File → Open` → выберите папку `Streaks`. Дождитесь окончания Gradle sync.
3. Выберите эмулятор (API 26+) или телефон по USB и нажмите **Run ▶**.

### Сборка APK
`Build → Build Bundle(s) / APK(s) → Build APK(s)`
или в терминале: `./gradlew assembleDebug`
→ `app/build/outputs/apk/debug/app-debug.apk` (установка: `adb install app-debug.apk`).

## Как пользоваться
1. Пролистайте onboarding (или нажмите Skip) → **Sign up** (имя, корректный email, пароль не короче 6 символов).
2. На экране **Today** нажмите **Add habit** или **+**, введите название, выберите иконку и периодичность,
   нажмите **Save habit**.
3. Нажмите на кружок привычки, чтобы отметить выполнение; нажмите на карточку, чтобы увидеть streak,
   best streak и последние 7 дней.
4. **Calendar** и **Stats** обновляются автоматически. В **Profile → Settings** есть тёмная тема и время напоминания.
5. Закройте и снова откройте приложение — данные и вход сохранятся. **Profile → Log out** возвращает на экран входа.

## Известные ограничения
- Время напоминания и переключатель напоминаний сохраняются, но системные уведомления пока не планируются.
- Аккаунты хранятся только на устройстве (демо-версия; пароль хранится в виде хэша SHA-256).
