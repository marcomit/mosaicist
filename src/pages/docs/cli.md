---
layout: ../../layouts/DocsLayout.astro
title: CLI 
description: Mosaic CLI
---
# CLI

Command-line tool for project and module management.

## Project Commands

```bash
# Create new project
mosaic init <name>

# Show project status
mosaic status

# Execute command across all tesserae
mosaic walk "<command>" -o  # -o shows output
```

## Tessera (Module) Commands

```bash
# List all tesserae
mosaic tessera list
mosaic tessera list --deps        # Show dependencies
mosaic tessera list --path rel    # Show relative paths

# Add new tessera
mosaic tessera add <name>

# Enable/disable tessera
mosaic tessera enable <name>
mosaic tessera disable <name>

# Remove tessera
mosaic tessera remove <name>
```

## Dependency Management

```bash
# Add dependency
mosaic deps add <tessera> <dependency>

# Remove dependency
mosaic deps remove <tessera> <dependency>

# List dependencies
mosaic deps list <tessera>
```

## Profile System

```bash
# List profiles
mosaic profile list

# Add profile
mosaic profile add <name> <tessera1> <tessera2> ...

# Switch profile
mosaic profile switch <name>

# Set default tessera for profile
mosaic profile default <profile> <tessera>
```

## Execution Commands

```bash
# Run profile
mosaic run <profile>

# Build profile
mosaic build <profile>

# Execute custom command
mosaic exec <command> <profile>

# Sync and generate init files
mosaic sync <profile>
```

## Code Generation

```bash
# Generate type-safe event classes
mosaic events generate
```
