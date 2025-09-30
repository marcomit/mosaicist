---
layout: ../../layouts/DocsLayout.astro
title: Events
description: Internal events
---


# Events

Type-safe event system for decoupled inter-module communication.

## Basic Usage

```dart
// Listen to events
events.on<UserData>('user/profile/updated', (context) {
  print('Profile: ${context.data?.name}');
});

// Emit events
events.emit<UserData>('user/profile/updated', userData);

// Remove listener
events.deafen(listener);
```

## Wildcard Patterns

**Single segment wildcard (`*`):**
```dart
events.on<dynamic>('user/*/updated', (context) {
  print('Updated: ${context.params[0]}'); // params[0] = matched segment
});
```

**Global wildcard (`#`):**
```dart
events.on<dynamic>('user/#', (context) {
  print('All segments: ${context.params}');
});
```

## Event Chains

```dart
class UserEvents extends Segment {
  UserEvents() : super('user');
}

final userEvents = UserEvents();

// Chain paths
userEvents.$('profile').$('avatar').emit<String>('url');

// Listen to chains
userEvents.$('profile').$('*').on<dynamic>((ctx) {
  print(ctx.params[0]);
});
```

## Retained Events

Events persist for late subscribers:

```dart
// Emit retained
events.emit<bool>('app/ready', true, true);

// Late subscriber receives it
events.on<bool>('app/ready', (ctx) {
  print('Received: ${ctx.data}');
});
```
