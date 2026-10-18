# Lab 2 — submission notes

## Deliverables
- Figma (public view link): https://www.figma.com/design/2nH2vz6hiFV0LiKBJKQfIf
- Source code: <GitHub repo link>
- APK: `app-debug.apk` (attach to a GitHub Release or the submission)
- Run / usage instructions: `README.md`

## Idea (EN)
Streaks is a mobile habit tracker. Users register, create daily or weekly habits with a reminder, mark them as
done each day and follow their progress through a streak counter, a monthly calendar and statistics.
The goal is to make building routines simple and motivating.

## Идея (RU)
Streaks — мобильный трекер привычек. Пользователь регистрируется, создаёт ежедневные или еженедельные привычки
с напоминанием, отмечает их выполнение каждый день и отслеживает прогресс с помощью серии дней подряд
(streak), календаря и статистики. Цель приложения — сделать формирование полезных привычек простым и мотивирующим.

## Figma: how the requirements are met
- 14 screens (min. 10): Splash, Onboarding, Sign up (error), Sign up, Log in, Home (empty), Add habit, Home,
  Habit details, Edit habit, Calendar, Statistics, Profile, Settings.
- Interactive prototype: 42 links; main flow Splash → Onboarding → Sign up → Home (empty) → Add habit → Home → Habit details.
- Single style: color variables, text styles, one icon set (Lucide-style outline icons), Inter font.
- States: button default/pressed/disabled, input default/focused/filled/error, empty list, checked/unchecked habit,
  selected icon / segmented control, toggle on/off.
- Components: Button (6 variants), Input (4 variants), NavBar (4 variants).

## Android: how the requirements are met
- Design match: same palette, radii, spacing, icons (vector drawables), 11 fragments for 14 designed screens.
- Navigation: Navigation Component + BottomNavigationView.
- Forms with validation: sign up, log in, habit form.
- Lists: habit list (RecyclerView + ListAdapter), calendar grid, stats rows.
- Persistence: Room + SharedPreferences.
- Architecture: MVVM, ViewModel + StateFlow, repository-style DAO access.
