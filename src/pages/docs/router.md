---
layout: ../../layouts/DocsLayout.astro
title: Router
description: How mosaic handles navigation
---


# Router

Navigation system supporting both inter-module and intra-module routing.

## Inter-Module Navigation

Switch between modules:

```dart
// Navigate to module
await router.go<String>('dashboard');

// With data
await router.go<UserData>('profile', currentUser);

// Go back
router.goBack<String>('result');
```

## Intra-Module Navigation

Navigate within a module's internal stack:

```dart
// Push page
final result = await router.push<String>(EditProfilePage());

// Pop with result
router.pop<String>('saved');

// Clear stack
router.clear();
```

## Context Extensions

Convenient navigation from widgets:

```dart
// Navigate to module
context.go('settings');

// Push page
final result = await context.push<bool>(EditPage());

// Pop
context.pop(true);
```

## Stack Management

```dart
// Check stack depth
final depth = router.stackDepth;

// Check if can pop
if (router.canPop) {
  router.pop();
}
```
