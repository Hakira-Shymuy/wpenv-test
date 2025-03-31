# wp-env Project Structure Guide

This document provides a detailed breakdown of the `wp-env` package structure to help understand how it works and assist in creating your own version.

## Entry Points and Core Files

### 1. `/bin/wp-env`
- **Purpose**: Main entry point script
- **Functionality**: 
  - Bootstraps the CLI
  - Looks for local installations of `wp-env` before using the global one
  - Passes commands to the CLI module

### 2. `/lib/cli.js`
- **Purpose**: Command-line interface setup
- **Functionality**:
  - Configures `yargs` for command-line argument parsing
  - Defines all available commands (start, stop, clean, etc.)
  - Handles errors and provides spinner feedback
  - Wraps command execution with error handling and progress indicators

### 3. `/lib/env.js`
- **Purpose**: Main package export
- **Functionality**:
  - Exports all commands and error classes
  - Acts as the public API for the package

## Command Implementations

### 4. `/lib/commands/index.js`
- **Purpose**: Exports all command implementations
- **Functionality**: Centralizes imports of individual command modules

### 5. `/lib/commands/start.js`
- **Purpose**: Starts the WordPress development environment
- **Functionality**:
  - Reads configuration
  - Downloads WordPress sources if needed
  - Configures Docker Compose
  - Starts Docker containers
  - Sets up WordPress (database, plugins, themes)
  - Executes lifecycle scripts

### 6. `/lib/commands/stop.js`
- **Purpose**: Stops the WordPress environment
- **Functionality**:
  - Stops and removes Docker containers while preserving data

### 7. `/lib/commands/clean.js`
- **Purpose**: Cleans WordPress databases
- **Functionality**:
  - Resets WordPress databases to their initial state

### 8. `/lib/commands/destroy.js`
- **Purpose**: Completely removes the environment
- **Functionality**:
  - Removes Docker containers, volumes, and networks
  - Deletes downloaded sources and configuration

### 9. `/lib/commands/run.js`
- **Purpose**: Runs commands in Docker containers
- **Functionality**:
  - Executes arbitrary commands in specified containers
  - Supports interactive shell sessions

### 10. `/lib/commands/logs.js`
- **Purpose**: Shows logs from Docker containers
- **Functionality**:
  - Displays and optionally watches logs from containers

### 11. `/lib/commands/install-path.js`
- **Purpose**: Shows the installation path
- **Functionality**:
  - Displays where environment files are stored

## Configuration Handling

### 12. `/lib/init-config.js`
- **Purpose**: Initializes configuration
- **Functionality**:
  - Loads and validates configuration from `.wp-env.json`
  - Sets up working directories
  - Prepares Docker Compose configuration

### 13. `/lib/config/load-config.js`
- **Purpose**: Loads configuration from files
- **Functionality**:
  - Reads `.wp-env.json` and `.wp-env.override.json` files
  - Caches configuration for performance

### 14. `/lib/config/parse-config.js`
- **Purpose**: Parses configuration files
- **Functionality**:
  - Validates and normalizes configuration
  - Resolves source paths and references

### 15. `/lib/config/validate-config.js`
- **Purpose**: Validates configuration
- **Functionality**:
  - Ensures configuration meets requirements
  - Provides helpful error messages

### 16. `/lib/build-docker-compose-config.js`
- **Purpose**: Generates Docker Compose configuration
- **Functionality**:
  - Creates Docker Compose YAML configuration
  - Sets up volumes, networks, and services
  - Configures port mappings and environment variables

## WordPress and Source Management

### 17. `/lib/wordpress.js`
- **Purpose**: WordPress-specific utilities
- **Functionality**:
  - Configures WordPress installation
  - Sets up database connections
  - Manages WordPress directories and files

### 18. `/lib/download-sources.js`
- **Purpose**: Downloads WordPress sources
- **Functionality**:
  - Handles various source types (git, zip, local)
  - Downloads WordPress core, plugins, and themes

### 19. `/lib/download-wp-phpunit.js`
- **Purpose**: Downloads WordPress PHPUnit test files
- **Functionality**:
  - Sets up PHPUnit testing environment

## Utility Functions

### 20. `/lib/cache.js`
- **Purpose**: Caching utilities
- **Functionality**:
  - Tracks changes to configuration and sources
  - Avoids unnecessary downloads and setups

### 21. `/lib/md5.js`
- **Purpose**: Creates MD5 hashes
- **Functionality**:
  - Generates checksums for caching and comparison

### 22. `/lib/retry.js`
- **Purpose**: Retry mechanism for operations
- **Functionality**:
  - Retries failed operations with backoff

### 23. `/lib/get-host-user.js`
- **Purpose**: Gets host user information
- **Functionality**:
  - Retrieves user ID and group ID for Docker container permissions

### 24. `/lib/parse-xdebug-mode.js`
- **Purpose**: Parses Xdebug configuration
- **Functionality**:
  - Validates and normalizes Xdebug mode settings

### 25. `/lib/execute-lifecycle-script.js`
- **Purpose**: Executes lifecycle scripts
- **Functionality**:
  - Runs user-defined scripts at specific points in the workflow

## Docker Integration

The package uses Docker and Docker Compose to create isolated environments:

1. **Docker Images**:
   - Uses MariaDB for the database
   - Uses WordPress images for web servers
   - Creates custom images for CLI tools

2. **Volume Mounting**:
   - Mounts local directories into containers
   - Ensures file permissions work correctly
   - Separates development and test environments

3. **Networking**:
   - Sets up port mappings for web access
   - Configures container networking

## Key Design Patterns

1. **Configuration First**: All operations are driven by configuration
2. **Caching**: Avoids redundant operations through caching
3. **Error Handling**: Comprehensive error handling with helpful messages
4. **Progress Indication**: Uses spinners to show progress
5. **Isolation**: Keeps development and test environments separate

## Performance Considerations

1. **Docker Operations**: Most time is spent in Docker operations
2. **Source Downloads**: Downloading sources can be slow
3. **Caching**: Caching is used to avoid redundant operations
4. **File System Operations**: Many file system operations can impact performance

## Potential Optimization Areas for Bun Implementation

1. **Faster JavaScript Execution**: Bun's faster startup and execution
2. **Improved File I/O**: Better file system performance
3. **Docker Integration**: Direct Docker API calls instead of wrappers
4. **Parallel Operations**: Better concurrency for downloads and setup
5. **Lighter Docker Images**: Optimized Docker images for faster startup
