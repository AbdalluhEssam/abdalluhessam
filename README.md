# Hi, I'm **Abdalluh Essam** — Mobile App Developer (Flutter)

[![Flutter](https://img.shields.io/badge/Flutter-02569B?logo=flutter\&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-0175C2?logo=dart\&logoColor=white)](https://dart.dev)
[![BLoC](https://img.shields.io/badge/State%20Mgmt-BLoC%20%26%20Cubit-7D4698)](https://bloclibrary.dev)
[![Clean Architecture](https://img.shields.io/badge/Architecture-Clean%20Architecture-1A73E8)](#-how-i-build)
[![Firebase](https://img.shields.io/badge/Backend-Firebase-FFCA28?logo=firebase\&logoColor=000)](https://firebase.google.com)
[![Dio](https://img.shields.io/badge/HTTP-Dio-000000)](https://pub.dev/packages/dio)
[![GetIt](https://img.shields.io/badge/DI-GetIt-0FA958)](https://pub.dev/packages/get_it)
[![CI](https://img.shields.io/badge/CI-GitHub%20Actions-2088FF?logo=github-actions\&logoColor=white)](https://github.com/features/actions)

> **3+ years** building production-ready, scalable mobile apps with Flutter. I love crafting clean architectures, robust state management with **BLoC/Cubit**, and smooth CI/CD.

---

## 🔗 Quick Links

* **Portfolio site:** *coming soon*
* **Resume/CV:** `[Add link here]`
* **Email:** `[your@email.com]`
* **LinkedIn:** `[Add link]` • **X/Twitter:** `[Add link]`

---

## 🧰 Tech Stack

* **Languages:** Dart, a bit of Kotlin/Swift for native bridges
* **Framework:** Flutter (Material 3, Responsive/Adaptive UIs)
* **State Management:** BLoC, Cubit
* **Architecture:** Clean Architecture, SOLID, Feature-first, Repository pattern
* **Networking:** Dio, Retrofit-style services, Interceptors, Error handling
* **Dependency Injection:** GetIt
* **Storage:** Firebase (Auth/Firestore/Storage), SQLite, Hive, Shared Preferences
* **Testing:** Unit, Widget, Integration (`integration_test`), Mockito, Golden tests
* **CI/CD:** GitHub Actions, Fastlane, Firebase App Distribution, Play Console, App Store Connect
* **Analytics/Crash:** Firebase Analytics, Crashlytics

---

## 🚀 Highlights

* Built and shipped multiple apps on **Google Play** and **Apple App Store** (feature-driven modules, offline-first where needed).
* Implemented **clean, testable** codebases using **BLoC/Cubit** with clear separation of concerns.
* Automated build pipelines with **GitHub Actions** → run tests, build APK/AAB/IPA, and distribute to testers.
* Strong focus on **UX performance**: lazy loading, caching, debouncing, and frame budget awareness.

---

## ⭐ Featured Projects

> Replace placeholders with your real repos and store/play links.

### 1) ADA Ecosystem — Notes App

* **Stack:** Flutter, Cubit/BLoC, Dio, GetIt, PHP REST API, MySQL
* **Features:** Auth, create/update notes, search, offline cache, dark mode
* **Repo:** `https://github.com/YOUR_GITHUB_USERNAME/ada-ecosystem`
* **Android:** `[Play link]` · **iOS:** `[App Store link]`

### 2) CourseHub — E‑Learning Client

* **Stack:** Flutter, BLoC, Clean Architecture, Firebase
* **Features:** Search with dynamic UI (switches to ListView on input), clear recent history, unified Course model
* **Repo:** `https://github.com/YOUR_GITHUB_USERNAME/coursehub`

### 3) KidsCare — Onboarding + Profile

* **Stack:** Flutter, Cubit, GetIt
* **Features:** 3‑page onboarding from List model, Edit Profile (Clean Architecture), conditional kids form (Yes/No)
* **Repo:** `https://github.com/YOUR_GITHUB_USERNAME/kidscare`

> *Want me to pin your top repositories here with cards?*
>
> ```md
> [![Readme Card](https://github-readme-stats.vercel.app/api/pin/?username=YOUR_GITHUB_USERNAME&repo=REPO_NAME)](https://github.com/YOUR_GITHUB_USERNAME/REPO_NAME)
> ```

---

## 🧪 Sample: BLoC/Cubit Pattern (Clean Architecture)

```dart
// domain/entities/note.dart
class Note {
  final String id;
  final String title;
  final String content;
  const Note({required this.id, required this.title, required this.content});
}

// domain/repositories/note_repo.dart
abstract class NoteRepo {
  Future<List<Note>> getNotes();
  Future<void> addNote(Note note);
}

// data/datasources/remote_note_ds.dart
class RemoteNoteDS {
  final Dio dio;
  RemoteNoteDS(this.dio);
  Future<List<Note>> fetchNotes() async {
    final res = await dio.get('/notes');
    return (res.data as List).map((j) => Note(
      id: j['id'].toString(),
      title: j['title'],
      content: j['content'],
    )).toList();
  }
}

// data/repositories/note_repo_impl.dart
class NoteRepoImpl implements NoteRepo {
  final RemoteNoteDS remote;
  NoteRepoImpl(this.remote);
  @override
  Future<List<Note>> getNotes() => remote.fetchNotes();
  @override
  Future<void> addNote(Note note) async {
    await remote.dio.post('/notes', data: {
      'title': note.title,
      'content': note.content,
    });
  }
}

// presentation/cubit/notes_cubit.dart
class NotesState {
  final List<Note> items;
  final bool loading;
  final String? error;
  const NotesState({this.items = const [], this.loading = false, this.error});
  NotesState copyWith({List<Note>? items, bool? loading, String? error}) =>
      NotesState(items: items ?? this.items, loading: loading ?? this.loading, error: error);
}

class NotesCubit extends Cubit<NotesState> {
  final NoteRepo repo;
  NotesCubit(this.repo) : super(const NotesState());

  Future<void> fetch() async {
    emit(state.copyWith(loading: true, error: null));
    try {
      final data = await repo.getNotes();
      emit(state.copyWith(items: data, loading: false));
    } catch (e) {
      emit(state.copyWith(loading: false, error: e.toString()));
    }
  }

  Future<void> add(String title, String content) async {
    try {
      await repo.addNote(Note(id: 'tmp', title: title, content: content));
      await fetch();
    } catch (e) {
      emit(state.copyWith(error: e.toString()));
    }
  }
}
```

---

## 🏗️ How I Build

* **Feature-first folders**: `features/<module>/domain | data | presentation`
* **SOLID** principles and clear boundaries
* **GetIt** for DI, **Dio** with interceptors for auth/logging/retry
* **Error handling** with typed failures & UI states
* **Testing** pyramid: unit → widget → integration (kept fast in CI)
* **Localization**: `easy_localization`
* **Theming**: M3, light/dark, semantic colors

```
lib/
  core/
  features/
    notes/
      data/
      domain/
      presentation/
  app.dart
  main.dart
```

---

## ⚙️ CI: GitHub Actions Example (Flutter)

```yml
name: Flutter CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: subosito/flutter-action@v2
        with:
          channel: stable
      - run: flutter --version
      - run: flutter pub get
      - run: flutter test --coverage
      - run: flutter build apk --release
      - uses: actions/upload-artifact@v4
        with:
          name: app-release-apk
          path: build/app/outputs/flutter-apk/app-release.apk
```

---

## 📊 GitHub Stats

> Replace `YOUR_GITHUB_USERNAME` with your username.

![Stats](https://github-readme-stats.vercel.app/api?username=YOUR_GITHUB_USERNAME\&show_icons=true)
![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=YOUR_GITHUB_USERNAME\&layout=compact)

---

## ✍️ Writing & Talks

* *Add links to any articles, talks, or demos*

---

## 🤝 Open Source

* PRs/issues welcome! See contribution guidelines in each repo.

---

## 📫 Contact

* Email: `[your@email.com]`
* LinkedIn: `[Add link]`
* Location: Open to remote/onsite opportunities

---

### License

This README and code snippets are available under the MIT License unless stated otherwise in specific repositories.

---

> **Tip:** Copy this `README.md` into your GitHub profile repo named **`YOUR_GITHUB_USERNAME/YOUR_GITHUB_USERNAME`** to show it on your profile page.
