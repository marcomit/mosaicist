---
layout: ../../layouts/DocsLayout.astro
title: Profiles
description: Mosaic profiles
---
# Profiles

Build configurations for different environments and feature sets.

## Overview

Profiles define:
- Which tesserae (modules) are active
- Default tessera for the profile
- Custom build/run commands
- Environment-specific configurations

## Configuration

**mosaic.yaml:**
```yaml
profiles:
  development:
    default: app
    tesserae: [app, auth, dashboard, debug_tools]
    commands:
      test: "flutter test"
    run: "flutter run"
    build: "flutter build apk"
    
  production:
    default: app
    tesserae: [app, auth, dashboard, analytics]
    run: "flutter run --release"
    build: "flutter build apk --release"
```

## Commands

```bash
# List profiles
mosaic profile list

# Switch active profile
mosaic profile switch production

# Show profile details
mosaic profile show development

# Add tessera to profile
mosaic profile add auth development

# Remove tessera from profile
mosaic profile remove debug_tools production

# Set default tessera
mosaic profile set-default app development

# Sync profile (generate init files)
mosaic profile sync development
```

## Execution

```bash
# Run with profile
mosaic run development

# Build with profile
mosaic build production

# Execute custom command
mosaic exec test development
```

## Use Cases

**Development Profile:**
- All modules enabled
- Debug tools included
- Fast hot reload

**Production Profile:**
- Only essential modules
- Analytics enabled
- Optimized builds

**Testing Profile:**
- Mock services
- Test utilities
- Specific test modules
```
