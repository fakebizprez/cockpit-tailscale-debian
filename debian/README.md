# Debian Packaging for Cockpit Tailscale

This document describes the Debian packaging support added to the cockpit-tailscale project.

## Overview

The `debian/` directory contains all the necessary files to build a Debian package (.deb) for the cockpit-tailscale application alongside the existing RPM packaging.

## Package Information

- **Package Name**: cockpit-tailscale
- **Version**: 0.0.6-1
- **Architecture**: all (architecture-independent)
- **Section**: admin
- **Priority**: optional
- **Maintainer**: Gerard Braad <me@gbraad.nl>

## Dependencies

- **Build Dependencies**: debhelper-compat (= 13), nodejs, npm, make, gettext
- **Runtime Dependencies**: cockpit-bridge
- **Recommends**: tailscale

## Building the Package

### Prerequisites

Install the required build dependencies:

```bash
sudo apt install -y debhelper devscripts build-essential nodejs npm make gettext
```

### Build Steps

1. Clone the repository:
```bash
git clone https://github.com/spotsnel/cockpit-tailscale.git
cd cockpit-tailscale
```

2. Build the Debian package:
```bash
make deb
```

This will create the following files in the parent directory:
- `cockpit-tailscale_0.0.6-1_all.deb` - The main package
- `cockpit-tailscale_0.0.6-1_amd64.buildinfo` - Build information
- `cockpit-tailscale_0.0.6-1_amd64.changes` - Changes file

### Manual Build

Alternatively, you can use dpkg-buildpackage directly:

```bash
dpkg-buildpackage -us -uc -b -d
```

## Installing the Package

```bash
sudo dpkg -i cockpit-tailscale_0.0.6-1_all.deb
sudo apt-get install -f  # Install missing dependencies if any
```

## Package Contents

The package installs the following files:
- `/usr/share/cockpit/tailscale/` - Cockpit application files (HTML, CSS, JS, fonts)
- `/usr/share/metainfo/org.cockpit-project.tailscale.metainfo.xml` - AppStream metadata
- `/usr/share/doc/cockpit-tailscale/` - Documentation files

## Usage

After installation, the Tailscale application will be available in the Cockpit web interface at `https://your-server:9090`. Look for "Tailscale" in the left navigation menu.

## Debian Packaging Files

The `debian/` directory contains:

- `control` - Package metadata and dependencies
- `rules` - Build rules and packaging instructions
- `changelog` - Package version history
- `copyright` - License and copyright information
- `cockpit-tailscale.docs` - Documentation files to include

## Maintenance

To update the package version:

1. Update the version in `debian/changelog`
2. Update the version in `package.json` if needed
3. Rebuild the package with `make deb`