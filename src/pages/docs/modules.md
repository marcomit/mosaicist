---
layout: ../../layouts/DocsLayout.astro
title: Modules
description: Understanding module lifecycle and management
---

# Modules

Modules are isolated, self-contained features with their own lifecycle, dependencies, and navigation.

## Creating a Module

```dart
class UserModule extends Module {
  UserModule() : super(name: 'user');

  @override
  Widget build(BuildContext context) {
    return UserHomePage();
  }

  @override
  Future<void> onInit() async {
    // Setup services
    di.put<UserRepository>(UserRepositoryImpl());
    di.put<AuthService>(AuthServiceImpl());
    
    // Load configuration
    await loadUserPreferences();
  }

  @override
  Future<void> onDispose() async {
    // Cleanup
    await saveUserState();
    await closeConnections();
  }
}
```

## Lifecycle States

```
Uninitialized → Initializing → Active → Suspended → Disposing → Disposed
                      ↓           ↓
                    Error ← ← ← ←
```

**States:**
- **Uninitialized** - Created but not initialized
- **Initializing** - `onInit()` running
- **Active** - Ready and operational
- **Suspended** - Temporarily inactive
- **Disposing** - `onDispose()` running
- **Disposed** - Permanently shut down
- **Error** - Failed during lifecycle transition

## Lifecycle Hooks

```dart
class MyModule extends Module {
  @override
  Future<void> onInit() async {
    // Called during initialization
    // Setup services, load configuration
  }

  @override
  void onActive(RouteTransitionContext ctx) {
    // Called when module becomes active
    // Refresh data, start timers
  }

  @override
  Future<void> onSuspend() async {
    // Called when suspended
    // Pause operations, save state
  }

  @override
  Future<void> onResume() async {
    // Called when resumed
    // Restart operations
  }

  @override
  Future<void> onDispose() async {
    // Called during cleanup
    // Release resources, save state
  }

  @override
  Future<void> onError(Object error, StackTrace trace) async {
    // Handle errors during lifecycle
    logger.error('Module error: $error');
  }

  @override
  Future<bool> onRecover() async {
    // Attempt recovery from error state
    return true; // Return false to prevent recovery
  }
}
```

## Module Registration

```dart
// Automatic via CLI-generated init.dart
await mosaic.registry.initialize(appModule, [
  authModule,
  dashboardModule,
  settingsModule,
]);
```

## Module Dependencies

```dart
class DashboardModule extends Module {
  DashboardModule() : super(name: 'dashboard');

  @override
  List<Module> get dependencies => [
    AuthModule(),      // Dashboard needs auth
    AnalyticsModule(), // Dashboard needs analytics
  ];
}
```

Dependencies are initialized in topological order automatically.

## Module Operations

```dart
// Check state
if (module.state == ModuleLifecycleState.active) {
  // Module is ready
}

// Suspend module
await module.suspend();

// Resume module
await module.resume();

// Dispose module
await module.dispose();

// Recover from error
final recovered = await module.recover();
```

## Internal Navigation

Each module has its own navigation stack:

```dart
// Push page
final result = await module.push<String>(EditPage());

// Pop with result
module.pop<String>('saved');

// Clear stack
module.clear();

// Stack info
final depth = module.stackDepth;
final canPop = module.canPop;
```

## Error Handling

```dart
class ResilientModule extends Module {
  @override
  Future<void> onInit() async {
    try {
      await riskyInitialization();
    } catch (e) {
      // Handle initialization error
      logger.error('Init failed: $e');
      rethrow; // Module enters error state
    }
  }

  @override
  Future<bool> onRecover() async {
    // Try to recover
    try {
      await fallbackInitialization();
      return true; // Recovery successful
    } catch (e) {
      return false; // Recovery failed
    }
  }
}
```

## Hot Reload (Development)

```dart
// Replace module with new implementation
await oldModule.hotReload(newModule);
```

Transfers state to new module without losing data.
