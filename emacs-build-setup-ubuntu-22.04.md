# Building Emacs 30.1 from Source on Ubuntu 22.04

This guide walks you through the complete process of setting up all required dependencies and building the latest version of GNU Emacs from source on Ubuntu 22.04 LTS.

## Prerequisites

- Ubuntu 22.04 LTS system with sudo privileges
- Internet connection for downloading packages and source code
- Approximately 2-3 GB of free disk space

## Quick Start

For those who want to jump straight to building, run these commands:

```bash
# Install all dependencies
sudo apt update
sudo apt install -y build-essential autoconf libgtk-3-dev libgnutls28-dev \
  libtiff5-dev libgif-dev librsvg2-dev libjpeg-dev libwebp-dev libxml2-dev \
  libpng-dev libxpm-dev libncurses-dev texinfo libjansson4 libjansson-dev \
  libgccjit0 libgccjit-12-dev libtree-sitter-dev

# Download and build Emacs 30.1
wget https://ftp.gnu.org/gnu/emacs/emacs-30.1.tar.gz
tar xzf emacs-30.1.tar.gz
cd emacs-30.1
./configure --with-native-compilation --with-json --with-tree-sitter
make -j$(nproc)
sudo make install
```

## Detailed Setup Process

### Step 1: System Update

First, ensure your system is up to date:

```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Build Dependencies

#### Essential Build Tools

Install the basic compilation tools:

```bash
sudo apt install -y build-essential autoconf texinfo
```

#### Core Emacs Dependencies

Install libraries required for basic Emacs functionality:

```bash
sudo apt install -y \
  libgtk-3-dev \
  libgnutls28-dev \
  libxml2-dev \
  libncurses-dev
```

#### Image and Graphics Support

For full image format support:

```bash
sudo apt install -y \
  libtiff5-dev \
  libgif-dev \
  librsvg2-dev \
  libjpeg-dev \
  libwebp-dev \
  libpng-dev \
  libxpm-dev
```

#### Modern Emacs 30.1 Features

For native compilation, JSON support, tree-sitter, and webkit support:

```bash
sudo apt install -y \
  libjansson4 \
  libjansson-dev \
  libgccjit0 \
  libgccjit-12-dev \
  libtree-sitter-dev \
  sqlite3 \
  libwebkit2gtk-4.0-dev \
  imagemagick
```

#### Complete Dependency Installation (One Command)

Alternatively, install all dependencies at once:

```bash
sudo apt install -y \
  build-essential \
  autoconf \
  texinfo \
  libgtk-3-dev \
  libgnutls28-dev \
  libtiff5-dev \
  libgif-dev \
  librsvg2-dev \
  libjpeg-dev \
  libwebp-dev \
  libxml2-dev \
  libpng-dev \
  libxpm-dev \
  libncurses-dev \
  libjansson4 \
  libjansson-dev \
  libgccjit0 \
  libgccjit-12-dev \
  libtree-sitter-dev \
  sqlite3 \
  libwebkit2gtk-4.0-dev \
  imagemagick
```

### Step 3: Download Emacs Source Code

#### Option A: Download Release Tarball (Recommended)

Download the official Emacs 30.1 release:

```bash
# Create working directory
mkdir -p ~/emacs-build && cd ~/emacs-build

# Download Emacs 30.1
wget https://ftp.gnu.org/gnu/emacs/emacs-30.1.tar.gz
wget https://ftp.gnu.org/gnu/emacs/emacs-30.1.tar.gz.sig

# Verify signature (optional but recommended)
gpg --keyserver keyserver.ubuntu.com --recv-keys 28D3BED851FDF3AB57FEF93C233587A47C207910
gpg --verify emacs-30.1.tar.gz.sig emacs-30.1.tar.gz

# Extract source code
tar xzf emacs-30.1.tar.gz
cd emacs-30.1
```

#### Option B: Clone from Git Repository

For the latest development version:

```bash
# Clone Emacs repository
git clone https://git.savannah.gnu.org/git/emacs.git
cd emacs

# Switch to stable branch (optional)
git checkout emacs-30

# Generate configure script
./autogen.sh
```

### Step 4: Configure Build

Set up the build environment:

```bash
# Set compiler (if needed)
export CC=/usr/bin/gcc-12
export CXX=/usr/bin/g++-12

# Configure with recommended options
./configure \
  --with-native-compilation \
  --with-json \
  --with-tree-sitter \
  --with-pgtk \
  --with-rsvg \
  --with-gnutls \
  --with-mailutils \
  --with-sqlite3 \
  --with-webp
```

#### Configuration Options Explained

- `--with-native-compilation`: Enables native compilation for better performance
- `--with-json`: Adds JSON parsing support (enhanced in Emacs 30.1)
- `--with-tree-sitter`: Enables tree-sitter for better syntax highlighting
- `--with-pgtk`: Pure GTK support (better Wayland compatibility)
- `--with-rsvg`: SVG image support
- `--with-gnutls`: TLS/SSL support
- `--with-mailutils`: Email handling support
- `--with-sqlite3`: SQLite database support (new in Emacs 30.1)
- `--with-webp`: WebP image format support

#### Minimal Configuration

For a basic build without modern features:

```bash
./configure --with-x-toolkit=gtk3
```

#### No-X Configuration

For a terminal-only build:

```bash
./configure --with-x-toolkit=no --without-x
```

### Step 5: Compile Emacs

Build Emacs using all available CPU cores:

```bash
make -j$(nproc)
```

**Note**: Compilation with native compilation enabled may take 10-30 minutes depending on your system. Emacs 30.1 includes improved native compilation performance.

### Step 6: Test the Build

Before installing, test that Emacs works correctly:

```bash
# Test in GUI mode
./src/emacs

# Test in terminal mode
./src/emacs -nw
```

### Step 7: Install Emacs

Install Emacs system-wide:

```bash
sudo make install
```

By default, this installs Emacs to `/usr/local/bin/emacs`.

### Step 8: Verify Installation

Check that Emacs is properly installed:

```bash
# Check version
emacs --version

# Check installation path
which emacs

# Launch Emacs
emacs
```

## Post-Installation

### Setting Up PATH

If `/usr/local/bin` is not in your PATH, add it to your shell configuration:

```bash
# For bash users
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# For zsh users
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Desktop Integration

Create a desktop entry for GUI access:

```bash
sudo tee /usr/share/applications/emacs.desktop > /dev/null <<EOF
[Desktop Entry]
Name=Emacs
GenericName=Text Editor
Comment=Edit text
MimeType=text/english;text/plain;text/x-makefile;text/x-c++hdr;text/x-c++src;text/x-chdr;text/x-csrc;text/x-java;text/x-moc;text/x-pascal;text/x-tcl;text/x-tex;application/x-shellscript;text/x-c;text/x-c++;
Exec=/usr/local/bin/emacs %F
Icon=emacs
Type=Application
Terminal=false
Categories=Development;TextEditor;
StartupWMClass=Emacs
Keywords=Text;Editor;
EOF
```

## Troubleshooting

### Common Issues

#### Missing libgccjit

If you encounter libgccjit errors:

```bash
# Install the correct version
sudo apt install libgccjit0 libgccjit-12-dev gcc-12

# Set compiler explicitly
export CC=gcc-12
./configure --with-native-compilation
```

#### GTK-related Errors

For GTK issues:

```bash
# Install additional GTK development packages
sudo apt install libgtk-3-dev libgtk-3-0 gtk+3.0

# Or build without GTK
./configure --with-x-toolkit=no
```

#### Permission Errors

If you encounter permission issues during installation:

```bash
# Install to user directory instead
./configure --prefix="$HOME/.local"
make install

# Add to PATH
export PATH="$HOME/.local/bin:$PATH"
```

### Build Errors

#### Autogen Issues

If `./autogen.sh` fails:

```bash
sudo apt install autotools-dev automake
./autogen.sh
```

#### Make Errors

For compilation errors:

```bash
# Clean and retry
make clean
make -j1  # Single-threaded for easier debugging
```

### Performance Issues

#### Slow Native Compilation

Native compilation can be resource-intensive:

```bash
# Reduce parallel jobs
make -j2

# Or disable native compilation
./configure --without-native-compilation
```

## Uninstallation

To remove the compiled Emacs:

```bash
cd ~/emacs-build/emacs-30.1  # or your build directory
sudo make uninstall
```

## Alternative Installation Methods

### Using build-dep

Install all build dependencies automatically:

```bash
sudo apt build-dep emacs
sudo apt install libgccjit-12-dev libtree-sitter-dev sqlite3 libwebkit2gtk-4.0-dev
```

### Minimal Dependencies

For a basic build with minimal features:

```bash
sudo apt install build-essential libgtk-3-dev libgnutls28-dev libgif-dev texinfo
```

## Version Information

This guide is specifically for:
- **Ubuntu**: 22.04 LTS (Jammy Jellyfish)
- **Emacs**: 30.1 (latest stable release - February 2025)
- **GCC**: 12.x (default in Ubuntu 22.04)

## What's New in Emacs 30.1

Emacs 30.1 includes several notable improvements:
- Enhanced native JSON support without external libjansson dependency
- Improved Android port with touch-screen support
- New tree-sitter-based major modes (elixir-ts-mode, heex-ts-mode, html-ts-mode, lua-ts-mode, php-ts-mode)
- Better native compilation performance
- SQLite database support
- Security fixes for CVE-2025-1244 and CVE-2024-53920

## Additional Resources

- [Official Emacs Build Instructions](https://www.gnu.org/software/emacs/manual/html_node/efaq/Installing-Emacs.html)
- [Emacs Wiki: Building Emacs](https://www.emacswiki.org/emacs/BuildingEmacs)
- [Ubuntu Emacs Packages](https://packages.ubuntu.com/search?keywords=emacs)

---

**Created for PC commissioning documentation - New Employee Onboarding**