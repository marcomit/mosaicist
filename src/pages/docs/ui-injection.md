---
layout: ../../layouts/DocsLayout.astro
title: UI Injection 

---


# UI Injection

Dynamically inject UI components across modules.

## Injecting Components

From source module:

```dart
class ProfileModule extends Module {
  @override
  void onInit() {
    injector.inject(
      'dashboard/sidebar',
      ModularExtension(
        (context) => ListTile(
          leading: Icon(Icons.person),
          title: Text('Profile'),
          onTap: () => context.go('profile'),
        ),
        priority: 1,
      ),
    );
  }
}
```

## Receiving Components

In target module:

```dart
class DashboardSidebar extends ModularStatefulWidget {
  const DashboardSidebar({Key? key}) : super(key: key);

  @override
  String get injectionKey => 'dashboard/sidebar';

  @override
  Widget build(BuildContext context, List<Widget> injected) {
    return ListView(
      children: [
        ...defaultItems,
        ...injected, // Injected components
      ],
    );
  }
}
```

## Priority System

Components are ordered by priority (higher first):

```dart
injector.inject('menu', ModularExtension(widget, priority: 10));
injector.inject('menu', ModularExtension(widget, priority: 5));
// First component renders before second
```

## Removing Injections

```dart
// Remove specific injection
injector.remove('dashboard/sidebar', extensionId);

// Clear all for key
injector.clear('dashboard/sidebar');
```
