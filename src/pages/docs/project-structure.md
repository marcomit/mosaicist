---
layout: ../../layouts/DocsLayout.astro
title: Project structure
---
# Project Structure

After running `mosaic init <name>`, the following structure is created:

```
<name>/
├── mosaic.yaml          # Project configuration
├── lib/
│   └── init.dart        # Auto-generated initialization file
└── tesserae/            # Modules directory
```

## Configuration File

**mosaic.yaml** contains:
- Project metadata (name, description, version)
- Default module/tessera
- Event definitions
- Profile configurations

**tesserae/** - Directory where modules (tesserae) are added and managed

**lib/init.dart** - Auto-generated file that initializes all active modules in topological order based on dependencies
