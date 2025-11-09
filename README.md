# zenbal
**ZEN BAsh Library** - A comprehensive cross-platform Bash utility library

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Bash](https://img.shields.io/badge/Bash-4.0%2B-brightgreen.svg)](https://www.gnu.org/software/bash/)
[![Platform Support](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Unix-blue.svg)](https://github.com/ergenius/zenbal)

The ZEN BAsh Library (zenbal) is a feature-rich bash script that acts as a comprehensive utility library for shell scripting. It provides an extensive set of functions that make bash scripting more efficient, readable, maintainable, and truly cross-platform.

## Key Features

- **Rich Terminal Output**: Beautiful colored output with icons, headers, and formatted messages
- **String Manipulation**: Comprehensive string processing and manipulation utilities  
- **Random Generation**: Generate random numbers, strings, and unique identifiers
- **File Operations**: Safe file handling, temporary file management, and content manipulation
- **Network Utilities**: Robust file downloading with automatic tool detection (curl/wget)
- **Advanced OS Detection**: Comprehensive cross-platform OS and distribution detection
- **Package Management**: Universal package installation across Linux distributions and macOS
- **Map Data Structures**: Full key-value mapping functionality with persistence
- **Command Utilities**: Command existence checking and automatic installation

- **Assert Functions**: File, directory, and system validation utilities
- **Development Tools**: Specialized functions for Erlang and other development workflows

## Installation

### Manual Installation

Download zenbal directly from the repository:

```bash
# Download the latest version
curl -O https://raw.githubusercontent.com/ergenius/zenbal/main/zenbal

# Make it executable
chmod +x zenbal
```

### Package Auto-Installation

Enable automatic installation of missing packages:

```bash
export ZEN_AUTOINSTALL_MISSING_PACKAGES=true
```

When enabled, `zen_ensure_command` will automatically install missing packages using your system's package manager.

## Advanced Usage Examples

### Cross-Platform Script Setup

```bash
#!/bin/bash
# Modern cross-platform bash script with zenbal

# Source zenbal with auto-installation enabled
export ZEN_AUTOINSTALL_MISSING_PACKAGES=true
source ./zenbal

# Script will automatically display environment info

zen_echo_h1 "My Cross-Platform Application"

# Ensure required commands exist (auto-install if needed)
zen_ensure_command "git" "Git Version Control"
zen_ensure_command "curl" "cURL HTTP Client" 
zen_ensure_command "jq" "JSON Processor"

zen_echo_success "All dependencies verified!"
```

### Configuration Management with Maps

```bash
#!/bin/bash
source ./zenbal

# Initialize configuration map
zen_map_init "app_config"

# Load configuration values
zen_map_set "app_config" "api_url" "https://api.example.com"
zen_map_set "app_config" "api_key" "secret_key_123"
zen_map_set "app_config" "debug_mode" "true"
zen_map_set "app_config" "max_retries" "3"

# Use configuration throughout script
zen_map_get "app_config" "api_url"
API_URL=$ZEN_FUNC_RESULT

zen_map_get "app_config" "debug_mode"
if [[ "$ZEN_FUNC_RESULT" == "true" ]]; then
    zen_echo_warning "Debug mode is enabled"
fi

# Display all configuration
zen_echo_h2 "Application Configuration"
zen_map_print "app_config"
```

### Robust File Processing

```bash
#!/bin/bash
source ./zenbal

# Safe temporary file handling
zen_file_tmp_name
TEMP_CONFIG=$ZEN_FUNC_RESULT

zen_echo_progress "Processing configuration file..."

# Download and process configuration
zen_net_download_url_if_file_not_found \
    "https://example.com/config.json" "$TEMP_CONFIG"

# Validate file exists
zen_assert_file_exist "$TEMP_CONFIG"

# Read and process
zen_file_read_content "$TEMP_CONFIG"
CONFIG_DATA=$ZEN_FUNC_RESULT

zen_echo_success "Configuration loaded: ${#CONFIG_DATA} characters"

# Clean up
rm -f "$TEMP_CONFIG"
```

### Package Management Automation

```bash
#!/bin/bash
source ./zenbal

zen_echo_h1 "Development Environment Setup"

# Update system packages
zen_echo_progress "Updating system packages..."
zen_pkg_upgrade

# Install development tools
REQUIRED_PACKAGES=("git" "curl" "wget" "vim" "htop" "tree")

for package in "${REQUIRED_PACKAGES[@]}"; do
    zen_echo_progress "Installing $package..."
    zen_pkg_install "$package"
done

# macOS specific tools
if [[ "$ZEN_OS_BASED_ON" == "macos" ]]; then
    zen_ensure_brew_app "cask" "visual-studio-code" "VS Code"
    zen_ensure_brew_app "formula" "node" "Node.js"
fi

zen_echo_success "Development environment setup complete!"
```

## Usage

### Basic Usage

Include zenbal in your bash script:

```bash
#!/bin/bash

# Source the zenbal library
source ./zenbal

# Now you can use zenbal functions
zen_echo_success "Hello, World!"
zen_echo_error "Something went wrong"
```

### Quick Examples

```bash
# Rich colored output with icons
zen_echo_success "Installation completed successfully!"
zen_echo_error "Configuration file not found"
zen_echo_warning "Deprecated function usage detected"
zen_echo_progress "Downloading dependencies..."

# String manipulation
zen_string_lower "HELLO WORLD"
echo $ZEN_FUNC_RESULT  # Output: hello world

zen_string_beginswith "https://github.com" "https://"
echo $ZEN_FUNC_RESULT  # Output: true

# Random generation
zen_random_alphanumeric 12
echo $ZEN_FUNC_RESULT  # Output: aB3kX9mPqR7s

zen_random_int30 1 100
echo $ZEN_FUNC_RESULT  # Output: 42

# Cross-platform package management
zen_ensure_command "curl" "cURL HTTP client"
zen_pkg_install "git"
zen_pkg_upgrade

# Map data structures
zen_map_init "user_config"
zen_map_set "user_config" "name" "John Doe"
zen_map_set "user_config" "email" "john@example.com"
zen_map_get "user_config" "name"
echo $ZEN_FUNC_RESULT  # Output: John Doe

# File operations with safety
zen_file_tmp_name
echo $ZEN_FUNC_RESULT  # Output: /tmp/zen-abc123def.tmp

zen_assert_file_exist "/etc/passwd"
zen_file_read_content "config.txt"

# Network operations with fallbacks
zen_net_download_url "https://example.com/file.txt" "./downloaded.txt"
zen_net_download_url_if_file_not_found "https://releases.com/app.zip" "./app.zip"
```

## Function Reference

### Echo Functions (`zen_echo`)
Beautiful terminal output with colors, icons, and formatting:
- `zen_echo_red()`, `zen_echo_green()`, `zen_echo_yellow()`, `zen_echo_blue()`, `zen_echo_magenta()`, `zen_echo_cyan()` - Colored text output
- `zen_echo_h1()`, `zen_echo_h2()`, `zen_echo_h3()` - Header formatting with styles (inverted, underlined, italic)
- `zen_echo_success()`, `zen_echo_error()`, `zen_echo_warning()`, `zen_echo_progress()` - Status messages with icons
- `zen_echo_panic()` - Error message with script termination
- `zen_echo_repeat()` - Repeat string multiple times

### String Functions (`zen_string`)
Comprehensive string manipulation and processing:
- `zen_string_beginswith()` - Check if string starts with specified prefix
- `zen_string_explode()` - Split string by delimiter into array
- `zen_string_lower()` - Convert string to lowercase
- `zen_string_upper()` - Convert string to uppercase  
- `zen_string_replace_all()` - Replace all occurrences of substring

### Random Functions (`zen_random`)
Generate random numbers, strings, and identifiers:
- `zen_random_int30()` - Generate uniformly distributed 30-bit integer with optional range
- `zen_random_string()` - Generate random string from custom character set
- `zen_random_alphanumeric()` - Generate mixed-case alphanumeric string
- `zen_random_lowercase_alphanumeric()` - Generate lowercase alphanumeric string
- `zen_random_uppercase_alphanumeric()` - Generate uppercase alphanumeric string
- `zen_random_lowercase_alpha()` - Generate lowercase alphabetic string
- `zen_random_uppercase_alpha()` - Generate uppercase alphabetic string
- `gen_random_string_time_based()` - Generate time-based unique identifier

### File Functions (`zen_file`)
Safe file operations and temporary file management:
- `zen_file_tmp_name()` - Generate unique temporary file path
- `zen_file_replace_string()` - Safely replace string in file using sed
- `zen_file_read_content()` - Read first line of file with error handling
- `zen_file_setup_path_tmp()` - Initialize temporary directory structure

### Assert Functions (`zen_assert`)
System validation and requirement checking:
- `zen_assert_file_exist()` - Assert file exists or panic with error
- `zen_assert_directory_exist()` - Assert directory exists or panic with error

### Network Functions (`zen_net`)
Robust file downloading with automatic tool detection:
- `zen_net_download_url()` - Download file with curl/wget fallback
- `zen_net_download_url_if_file_not_found()` - Conditional download if target doesn't exist

### Command Functions (`zen_command` & `zen_ensure`)
Command availability checking and automatic installation:
- `zen_command_exist()` - Check if command exists in system PATH
- `zen_ensure_command()` - Ensure command exists, install automatically if configured
- `zen_ensure_brew_app()` - Ensure Homebrew package (formula/cask) is installed
- `zen_ensure_asdf_plugin()` - Ensure asdf plugin and version are available

### Package Management Functions (`zen_pkg`)
Universal cross-platform package management:
- `zen_pkg_install()` - Install package using OS-appropriate package manager
- `zen_pkg_upgrade()` - Update all packages on the system
- `zen_pkg_provides_command()` - Find which package provides a command
- **Supported Package Managers**: apt-get, yum/dnf, zypper, urpmi, pacman, apk, emerge, xbps-install, homebrew, nix-env

### Map Functions (`zen_map`)
Full-featured key-value data structures:
- `zen_map_init()` - Initialize new map
- `zen_map_set()` - Set key-value pair  
- `zen_map_get()` - Retrieve value by key
- `zen_map_has_key()` - Check if key exists
- `zen_map_remove()` - Remove key-value pair
- `zen_map_keys()` - Get all keys as array
- `zen_map_values()` - Get all values as array
- `zen_map_size()` - Get number of entries
- `zen_map_clear()` - Remove all entries
- `zen_map_print()` - Display map contents for debugging

### OS Detection Functions (`zen_os`)
Comprehensive cross-platform environment detection:
- `zen_os_function()` - Call OS-specific function variants automatically
- `zen_environment_info()` - Display comprehensive system information
- **Supported Systems**: Debian/Ubuntu, Red Hat/CentOS/Fedora, openSUSE, Arch Linux, Alpine Linux, Gentoo, Void Linux, NixOS, macOS, Mandriva/Mageia

### Development Functions (`zen_erlang`)
Specialized development utilities:
- `zen_erlang_random_short_node_name()` - Generate random Erlang node names

## Configuration

### Environment Variables

- **`ZEN_AUTOINSTALL_MISSING_PACKAGES`**: Allow automatic package installation (`true` or `1`)
- **`ZEN_PATH_TMP_PREFIX`**: Prefix for temporary files (default: `zen`)
- **`ZEN_PATH_TMP`**: Custom temporary directory path

### Return Values

Many functions return their results in the global variable **`ZEN_FUNC_RESULT`**. Always check this variable after calling functions that return values:

```bash
zen_string_lower "HELLO"
echo "Result: $ZEN_FUNC_RESULT"  # Output: hello

zen_map_get "mymap" "key1"
if [[ -n "$ZEN_FUNC_RESULT" ]]; then
    echo "Found value: $ZEN_FUNC_RESULT"
fi
```

### Global Variables Set by zenbal

After initialization, zenbal sets these informational variables:
- **`ZEN_OS_NAME`** - Operating system name (linux, darwin, etc.)
- **`ZEN_OS_KERNEL`** - Kernel version
- **`ZEN_OS_MACHINE_HARDWARE_NAME`** - Hardware architecture
- **`ZEN_OS_BASED_ON`** - Base OS family for package management
- **`ZEN_OS_DISTRO_ID`** - Distribution identifier
- **`ZEN_OS_DISTRO_CODENAME`** - Human-readable distribution name
- **`ZEN_OS_RELEASE`** - Distribution version/release

## Platform Support

### Fully Supported
- **Linux Distributions:**
  - Debian/Ubuntu family (apt-get)
  - Red Hat/CentOS/Fedora family (yum/dnf)
  - openSUSE (zypper)
  - Arch Linux (pacman)
  - Alpine Linux (apk)
  - Gentoo (emerge)
  - Void Linux (xbps-install)
  - NixOS (nix-env)
  - Mandriva/Mageia (urpmi)
- **macOS** (Homebrew)

### Partial Support
- Other Unix-like systems (core functions work, package management don't)

### Package Manager Detection

zenbal automatically detects and uses the appropriate package manager for your system:
- **Debian/Ubuntu**: `apt-get`
- **Red Hat/Fedora**: `dnf` (preferred) or `yum` (fallback)  
- **openSUSE**: `zypper`
- **Arch Linux**: `pacman`
- **Alpine**: `apk`
- **Gentoo**: `emerge`
- **Void Linux**: `xbps-install`
- **macOS**: `homebrew` (with automatic cask detection)
- **NixOS**: `nix-env`

## OS Detection Details

zenbal provides comprehensive cross-platform detection that goes far beyond basic `uname` checks:

### Detection Sources
- **Modern Systems**: `/etc/os-release` (systemd standard)
- **Legacy Support**: `/etc/redhat-release`, `/etc/debian_version`, `/etc/lsb-release`
- **Architecture**: Hardware platform detection (x86_64, arm64, etc.)
- **Kernel Information**: Detailed kernel version and release info

### Automatic Base OS Mapping
zenbal automatically maps distribution IDs to base families for package management:

```bash
# Examples of automatic detection
Ubuntu → debian (uses apt-get)
Fedora → redhat (uses dnf/yum)  
Manjaro → arch (uses pacman)
Elementary OS → debian (uses apt-get)
Rocky Linux → redhat (uses dnf/yum)
```

### Detection Output Example
```bash
source ./zenbal
# Automatically displays:
# ✓ Running on darwin (25.1.0 kernel, arm64 architecture)
# ✓ Distribution: macOS (macos v26.1) based on macos  
# ✓ Package management will use macos compatible tools
```

## Complete Function List

<details>
<summary>Click to expand full function reference (75+ functions)</summary>

### Core Output Functions (15)
`zen_echo_repeat`, `zen_echo_red`, `zen_echo_green`, `zen_echo_yellow`, `zen_echo_blue`, `zen_echo_magenta`, `zen_echo_cyan`, `zen_echo_h1`, `zen_echo_h2`, `zen_echo_h3`, `zen_echo_progress`, `zen_echo_success`, `zen_echo_warning`, `zen_echo_error`, `zen_echo_panic`

### String Manipulation (5)
`zen_string_beginswith`, `zen_string_explode`, `zen_string_lower`, `zen_string_upper`, `zen_string_replace_all`

### Random Generation (8) 
`zen_random_int30`, `zen_random_string`, `zen_random_alphanumeric`, `zen_random_lowercase_alphanumeric`, `zen_random_lowercase_alpha`, `zen_random_uppercase_alphanumeric`, `zen_random_uppercase_alpha`, `gen_random_string_time_based`

### File Operations (6)
`zen_file_setup_path_tmp`, `zen_file_tmp_name`, `zen_file_replace_string`, `zen_file_read_content`, `zen_assert_file_exist`, `zen_assert_directory_exist`

### Map Data Structures (10)
`zen_map_init`, `zen_map_set`, `zen_map_get`, `zen_map_has_key`, `zen_map_remove`, `zen_map_keys`, `zen_map_values`, `zen_map_size`, `zen_map_clear`, `zen_map_print`

### Network Operations (2)
`zen_net_download_url`, `zen_net_download_url_if_file_not_found`

### Command Management (4)
`zen_command_exist`, `zen_ensure_command`, `zen_ensure_brew_app`, `zen_ensure_asdf_plugin`

### Package Management (25+)
`zen_pkg_upgrade`, `zen_pkg_install`, `zen_pkg_provides_command` + 22 OS-specific variants

### System Detection (4)
`zen_os_function`, `zen_environment_info`, plus internal detection functions

### Development Utils (1)
`zen_erlang_random_short_node_name`

</details>

## Migration from Other Libraries

### From manual bash functions:
```bash
# Replace manual color codes
echo -e "\033[32mSuccess\033[0m"  # Old way
zen_echo_success "Success"        # zenbal way

# Replace manual OS detection  
if [[ "$OSTYPE" == "linux-gnu"* ]]; then  # Old way
zen_os_function "_install_package"         # zenbal way
```

### From platform-specific scripts:
zenbal eliminates the need for separate scripts per platform - write once, run anywhere.

## Important Notes

- **Bash 4.0+ Required**: Uses associative arrays and modern bash features
- **Sudo Access**: Package management functions may require sudo privileges  
- **Internet Access**: Package installation features need network connectivity
- **Return Values**: Most functions use `ZEN_FUNC_RESULT` global variable for return values
- **Error Handling**: `zen_echo_panic` functions will exit the script on critical errors

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Areas where contributions are especially valuable:
- Additional OS/distribution support
- Package manager mappings for new commands
- Performance optimizations
- Documentation improvements
- Test coverage expansion

Please feel free to submit a Pull Request.

## Support & Community

- **Issues**: [GitHub Issues](https://github.com/ergenius/zenbal/issues) for bug reports and feature requests
- **Discussions**: [GitHub Discussions](https://github.com/ergenius/zenbal/discussions) for questions and community support
- **Updates**: Watch the repository for updates and new releases

---

**zenbal** - Making bash scripting beautiful, portable, and powerful.
