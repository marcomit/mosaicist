---
layout: ../../layouts/DocsLayout.astro
title: Multithreading
---


# Multithreading & AutoQueue

Thread-safe primitives for concurrent operations.

## Mutex

Protects shared data from concurrent modification:

```dart
final userCache = Mutex<Map<String, User>>({});

// Safe read
final users = await userCache.get();

// Safe write
await userCache.set({...users, 'id': newUser});

// Compound operations
await userCache.use((users) async {
  users['id'] = await loadUser('id');
  await saveToDatabase(users);
});

// Manual lock/release
final data = await userCache.lock();
try {
  // Modify data
} finally {
  userCache.release();
}
```

## Semaphore

Controls concurrent access to resource pools:

```dart
// Allow only 1 concurrent operation
final semaphore = Semaphore(1);

Future<void> downloadFile(String url) async {
  await semaphore.lock();
  try {
    // Download file
  } finally {
    semaphore.release();
  }
}

// Allow 3 concurrent operations
final poolSemaphore = Semaphore(3);
```

## AutoQueue

Automatic retry queue for unreliable operations:

```dart
final queue = InternalAutoQueue(maxRetries: 3);

// Queue operation with auto-retry
final result = await queue.push<String>(() async {
  final response = await http.get(endpoint);
  if (response.statusCode != 200) {
    throw Exception('HTTP ${response.statusCode}');
  }
  return response.body;
});

// Failed operations retry automatically
// Max 3 retries before final failure
```

## Features

- **Thread-safe**: All operations protected by locks
- **Sequential processing**: Operations processed one at a time
- **Auto-retry**: Failed operations retry up to maxRetries
- **Type-safe**: Generic support for any return type
