# LXC Ubuntu Docker Template

A Python-based project that provides an LXC (Linux Containers) template for Ubuntu with Docker pre-configured and ready to use.

## Overview

This project creates a reusable LXC container template that streamlines the setup of Ubuntu containers with Docker already installed and configured. It's designed to reduce the time needed to provision new containerized development and production environments.

## Features

- **Quick Container Setup**: Pre-configured Ubuntu LXC template with Docker installed
- **Ready-to-Use Docker Environment**: Docker is already included and configured in the template
- **Automation**: Python-based scripts to automate container initialization and Docker setup
- **Ubuntu-based**: Built on Ubuntu for stability and broad compatibility
- **Proxmox Compatible**: The tar.zst template can be directly imported into Proxmox VE

## Requirements

- LXC (Linux Containers)
- Ubuntu system (or compatible Linux distribution)
- Python 3.x
- Docker (for use within containers)
- Proxmox VE (for Proxmox-specific deployment)

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/jdmasa/lxc-ubuntu-docker-template.git
   cd lxc-ubuntu-docker-template
   ```

2. Ensure you have LXC installed on your system:
   ```bash
   sudo apt-get install lxc lxc-templates
   ```

3. Run any setup scripts provided in the project:
   ```bash
   # Review the available scripts first
   ls -la
   ```

## Usage

### Creating a Container from the Template

To create a new LXC container using this template:

```bash
# Create a container named 'mycontainer' from the template
lxc-create -t ubuntu-docker -n mycontainer
```

### Starting and Accessing the Container

```bash
# Start the container
sudo lxc-start -n mycontainer -d

# Attach to the container
sudo lxc-attach -n mycontainer

# Stop the container
sudo lxc-stop -n mycontainer
```

### Using Docker Inside the Container

Once inside the container, you can use Docker as normal:

```bash
# List Docker images
docker images

# Run a container
docker run -it ubuntu:latest bash
```

### Proxmox VE Integration

#### Importing the tar.zst Template from GitHub Release

This template is available as a pre-built release and can be directly imported into Proxmox VE using the "Download from URL" feature.

##### Method 1: Via Proxmox Web UI (Recommended)

1. **Access the Templates Section**:
   - Log in to your Proxmox VE web interface
   - Click on your node in the left sidebar
   - Go to `Datacenter` → `Nodes` → `<Your-Node>`
   - Navigate to `Search` section or click on `Storage` and look for template options

2. **Download Template from URL**:
   - In the Proxmox node, go to the **Templates** section
   - Click the **Download from URL** button (or similar option depending on your Proxmox version)
   - In the dialog that appears, paste the release download URL:
     ```
     https://github.com/jdmasa/lxc-ubuntu-docker-template/releases/download/v1.0/ubuntu-docker.tar.zst
     ```
     > **Note**: Replace `v1.0` with the actual release tag. Check the [Releases page](https://github.com/jdmasa/lxc-ubuntu-docker-template/releases) for the latest version.
   
   - Set the **Filename** to something descriptive, e.g., `ubuntu-docker.tar.zst`
   - Click **Download**
   - Wait for the download to complete (this may take a few minutes depending on file size and network speed)

3. **Create a Container from the Template**:
   - Once downloaded, click the `Create CT` button in the top right corner
   - Choose the following settings:
     - **Hostname**: Enter your desired container name
     - **Resource**: Allocate CPU, RAM, and storage as needed
     - **Template**: Select the downloaded template from the dropdown (it should appear as `ubuntu-docker*`)
     - **Network**: Configure your network settings (IP, gateway, DNS)
   - Review the settings and click `Create`

4. **Start the Container**:
   - Once created, select the container from the left sidebar
   - Click the `Start` button in the top right corner
   - The container will now boot with Docker pre-installed

##### Method 2: Via Command Line (SSH into Proxmox Node)

1. **Download the template to the Proxmox cache directory**:
   ```bash
   # SSH into your Proxmox node
   ssh root@<proxmox-host>
   
   # Navigate to the template cache directory
   cd /var/lib/vz/template/cache/
   
   # Download the template using wget or curl
   wget https://github.com/jdmasa/lxc-ubuntu-docker-template/releases/download/v1.0/ubuntu-docker.tar.zst
   
   # Or using curl:
   curl -L -o ubuntu-docker.tar.zst https://github.com/jdmasa/lxc-ubuntu-docker-template/releases/download/v1.0/ubuntu-docker.tar.zst
   ```
   
   > **Note**: Replace `v1.0` with the actual release tag.

2. **Verify the download**:
   ```bash
   # List the template
   ls -lh /var/lib/vz/template/cache/ubuntu-docker.tar.zst
   
   # Verify file integrity (if checksum is provided)
   sha256sum ubuntu-docker.tar.zst
   ```

3. **Create a new Container**:
   ```bash
   # Create container with the template
   pct create <VMID> /var/lib/vz/template/cache/ubuntu-docker.tar.zst \
     -hostname <container-name> \
     -cores <cpu-cores> \
     -memory <ram-in-mb> \
     -storage <storage-name> \
     -net0 name=eth0,bridge=vmbr0,ip=dhcp
   ```
   
   Example:
   ```bash
   pct create 100 /var/lib/vz/template/cache/ubuntu-docker.tar.zst \
     -hostname docker-container \
     -cores 4 \
     -memory 4096 \
     -storage local-lvm \
     -net0 name=eth0,bridge=vmbr0,ip=dhcp
   ```

4. **Start the Container**:
   ```bash
   pct start 100
   ```

5. **Access the Container**:
   ```bash
   # Enter the container shell
   pct exec 100 /bin/bash
   
   # Or via the web interface: Select the container → Console tab
   ```

#### Verifying Docker in the Proxmox Container

Once the container is running, verify Docker is working:

```bash
# Enter the container
pct exec <VMID> /bin/bash

# Inside the container, check Docker status
docker --version
docker ps
docker run hello-world
```

#### Proxmox Container Management Examples

```bash
# List all containers
pct list

# Stop a container
pct stop 100

# Reboot a container
pct reboot 100

# Delete a container
pct destroy 100

# View container configuration
pct config 100

# Edit container configuration (e.g., increase CPU cores and RAM)
pct set 100 -cores 8 -memory 8192

# Get container status
pct status 100

# View container logs
pct log 100
```

## Getting the Download URL

To find the latest release download URL:

1. Visit the [GitHub Releases page](https://github.com/jdmasa/lxc-ubuntu-docker-template/releases)
2. Find the latest release
3. Look for the `ubuntu-docker.tar.zst` (or similar) asset
4. Right-click on the download link and copy the URL
5. Use this URL in the Proxmox "Download from URL" feature

The URL will typically follow this pattern:
```
https://github.com/jdmasa/lxc-ubuntu-docker-template/releases/download/<TAG>/ubuntu-docker.tar.zst
```

## Project Structure

```
lxc-ubuntu-docker-template/
├── README.md                    # This file
├── ubuntu-docker.tar.zst        # Pre-built template (tar.zst format)
├── usr/
│   └── share/
│       └── doc/                 # Documentation files
├── [Template scripts]           # LXC template configuration scripts
└── [Configuration files]        # Setup and configuration files
```

## Contributing

Contributions are welcome! Please feel free to:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add some improvement'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

## License

This project is provided as-is. Please check for any included license files in the repository.

## Support

For issues or questions:

1. Check existing [GitHub Issues](https://github.com/jdmasa/lxc-ubuntu-docker-template/issues)
2. Create a new issue with detailed information about your problem
3. Include relevant logs and system information

## Related Resources

- [LXC Documentation](https://linuxcontainers.org/)
- [Docker Documentation](https://docs.docker.com/)
- [Ubuntu Container Documentation](https://ubuntu.com/containers)
- [Proxmox VE Documentation](https://pve.proxmox.com/wiki/Main_Page)
- [Proxmox Container (LXC) Documentation](https://pve.proxmox.com/wiki/LXC_Container)
- [GitHub Releases](https://github.com/jdmasa/lxc-ubuntu-docker-template/releases)

## Troubleshooting

### LXC not found
Ensure LXC is installed: `sudo apt-get install lxc lxc-templates`

### Permission denied when running lxc commands
Most LXC commands require sudo privileges. Use `sudo` when necessary.

### Docker not available in container
Verify that Docker was successfully installed during template creation. You may need to restart the container or run additional setup steps.

### Proxmox: Template download fails
- Check your internet connection
- Verify the URL is correct and the release exists
- Ensure sufficient storage space in `/var/lib/vz/template/cache/`
- Check Proxmox system logs: `journalctl -xe`

### Proxmox: Container fails to start after importing template
- Check container logs: `pct log <VMID>`
- Verify sufficient system resources are available
- Review Proxmox system logs: `journalctl -xe`
- Ensure the template file is not corrupted: `tar -tzf /var/lib/vz/template/cache/ubuntu-docker.tar.zst > /dev/null`

### Proxmox: Cannot access Docker inside container
- Verify Docker was installed correctly: `pct exec <VMID> docker --version`
- Restart Docker daemon: `pct exec <VMID> systemctl restart docker`
- Check cgroup configuration in Proxmox documentation
- Verify unprivileged container settings if using unprivileged containers

---

**Last Updated**: June 2026
