---
layout: ../../layouts/DocsLayout.astro
title: Signals
description: Reactive state management without Flutter dependency
---

# Signals

Reactive state management without Flutter dependency.

## Basic Signals

```dart
final counter = Signal<int>(0);

// Watch changes
counter.watch((value) {
  print('Counter: $value');
});

// Update state
counter.state++; // Triggers watchers

// Get value
print(counter.state);
```

## Computed Signals

Auto-recalculate from source signal:

```dart
final name = Signal<String>('John');
final greeting = name.computed((value) => 'Hello, $value');

print(greeting.state); // "Hello, John"

name.state = 'Jane';
print(greeting.state); // "Hello, Jane"
```

## Combined Signals

```dart
final name = Signal<String>('John');
final age = Signal<int>(25);

final profile = name.combine(age, (name, age) => 
  '$name ($age years old)'
);

print(profile.state); // "John (25 years old)"
```

## Batch Updates

```dart
counter.batch((current) {
  counter.state = 1;
  counter.state = 2;
  counter.state = 3;
  return current;
}); // Single notification with value 3
```

## Cleanup

```dart
class MyModule extends Module {
  final counter = Signal<int>(0);

  @override
  void dispose() {
    counter.dispose();
    super.dispose();
  }
}
```

## Flutter Integration

```dart
class MyWidget extends StatefulWidget {
  @override
  State<MyWidget> createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> with StatefulSignal {
  final counter = Signal<int>(0);

  @override
  List<Signal> get signals => [counter];

  @override
  Widget build(BuildContext context) {
    return Text('Count: ${watch(counter)}');
  }
}
```
