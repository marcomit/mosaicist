---
layout: ../../layouts/DocsLayout.astro
title: Logger
description: Awesome logs in mosaic
---


# Logger

Production-ready logging with multi-level filtering and multiple dispatchers.

## Initialization

```dart
await logger.init(
  tags: ['app', 'network', 'auth'],
  dispatchers: [
    ConsoleDispatcher(),
    FileLoggerDispatcher(
      path: 'logs',
      fileNameRole: (tag) => '${tag}_${DateTime.now().day}.log',
    ),
  ],
  defaultTags: ['myapp', 'v1.0.0'],
);

// Set minimum level
logger.setLogLevel(LogLevel.warning);
```

## Basic Logging

```dart
logger.debug('Debug info', ['debug']);
logger.info('User logged in', ['auth']);
logger.warning('Cache miss', ['performance']);
logger.error('Connection failed', ['network']);
```

## Message Formatting

```dart
// Add wrappers
logger.addWrapper(Logger.addData);  // Timestamp
logger.addWrapper(Logger.addType);  // Log level
logger.addWrapper(Logger.addTags);  // Tag list

// Output: "2025-09-30T10:15:30Z info: [auth] User logged in"
```

## Loggable Mixin

```dart
class UserService with Loggable {
  @override
  List<String> get loggerTags => ['user', 'auth'];

  Future<void> login(String email) async {
    info('Login attempt: $email'); // Auto-tagged
    try {
      await authService.login(email);
      info('Login successful');
    } catch (e) {
      error('Login failed: $e');
    }
  }
}
```

## Log Levels

```dart
enum LogLevel {
  debug,    // Detailed diagnostic info
  info,     // General information
  warning,  // Warning conditions
  error,    // Error conditions
}

// Production: Only warnings and errors
logger.setLogLevel(LogLevel.warning);
```

## Custom Dispatchers

```dart
class CustomDispatcher extends LoggerDispatcher {
  CustomDispatcher() : super('custom');

  @override
  Future<void> log(String msg, LogType type, List<String> tags) async {
    // Custom logic
  }
}

logger.addDispatcher(CustomDispatcher());
```
