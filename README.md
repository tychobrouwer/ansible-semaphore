# Semaphore Ansible Role

This Ansible role installs and configures [Semaphore](https://semaphoreui.com/),  a lightweight and modern web-based terminal for managing servers and applications. The role downloads the latest Semaphore binary, sets up a systemd service, and configures the application for reliable operation.

---

## Role Overview

The role performs the following tasks:

- Installs Semaphore on a Debian-based system.
- Sets up Semaphore as a systemd service for reliable operation and management.

---

## Requirements

- **Operating System**: Debian-based system (e.g., Debian 11/12, Ubuntu 20.04/22.04).
- **Ansible**: Version 2.9 or higher.
- **Privileges**: Root or sudo access on the target host(s).
- **Dependencies**:
  - `wget` or `curl` (for downloading Semaphore binaries).
  - `tar` (for extracting the Semaphore archive).
  - A working systemd installation (default on most modern Debian systems).

No additional Python modules or external roles are required beyond a standard Ansible setup.

---

## Role Variables

The role uses several variables to customize the installation and configuration of Semaphore. Below are the available variables with their defaults (if applicable):

| Variable                     | Description                                                                 | Default Value                 |
|------------------------------|-----------------------------------------------------------------------------|-------------------------------|
| `semaphore_arch`              | Target architecture for Semaphore (e.g., `amd64`, `arm64`).                | `amd64`                       |
| `semaphore_dir`               | Directory where the Semaphore binary will be installed.                    | `/usr/local/bin`              |
| `semaphore_config_template`   | Path to a custom Jinja2 template for the static configuration file.        | `templates/semaphore.json.j2` |

---

## Example Playbook

Here’s an example Ansible playbook demonstrating how to use this role:

```yaml
---
- hosts: servers
  roles:
    - role: tychobrouwer.semaphore  # Default installation
    
    - role: tychobrouwer.semaphore
      semaphore_arch: "amd64"
      semaphore_dir: "/usr/local/bin"
      semaphore_config_template: "/templates/semaphore.json.j2"
```

---

## Installation Steps (Performed by the Role)

1. **Download Semaphore**: Fetches the latest version of Semaphore for the target architecture from the official release page.
2. **Extract and Install**: Extracts the Semaphore binary and places it in `semaphore_dir` (e.g., `/usr/local/bin`).
3. **Configure Semaphore**:
   - Copies the rendered `semaphore_config_template` to `/etc/semaphore/config.json`.
4. **Set Up Systemd Service**: Creates and enables a systemd service to manage Semaphore, pointing to the configuration file.
5. **Start Service**: Ensures Semaphore is running and configured to load dynamic settings.

---

## Usage Notes

- **Custom Configuration**:
  - Create `semaphore.json.j2` for a custom static configuration file.
  - Example static config:

    ```json

    ```

- **Verification**: After running the playbook, verify Semaphore is operational by checking the logs (`journalctl -u semaphore`) or accessing a configured route.
