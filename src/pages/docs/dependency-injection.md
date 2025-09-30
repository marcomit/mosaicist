---
layout: ../../layouts/DocsLayout.astro
title: Dependency Injection
---


# Dependency Injection

Each module has an isolated DI container.

## Registration Types

```dart
class MyModule extends Module {
  @override
  Future<void> onInit() async {
    // Singleton
    di.put<DatabaseService>(DatabaseServiceImpl());
    
    // Factory (new instance each call)
    di.factory<HttpClient>(() => HttpClient());
    
    // Lazy singleton (created on first access)
    di.lazy<CacheService>(() => CacheService());
  }
}
```

## Retrieving Dependencies

```dart
// Get instance
final db = di.get<DatabaseService>();

// Check if registered
if (di.has<UserService>()) {
  final user = di.get<UserService>();
}
```

## Override for Testing

```dart
void main() {
  test('module with mock', () async {
    final module = MyModule();
    
    // Override dependency
    module.di.override<UserRepository>(MockUserRepository());
    
    await module.initialize();
  });
}
```

## Removing Dependencies

```dart
// Remove specific
di.remove<DatabaseService>();

// Clear all
di.clear();
```
