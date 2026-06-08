# LXC Ubuntu Docker Template

A Python-based project that provides an LXC (Linux Containers) template for Ubuntu with Docker pre-configured and ready to use.

## Overview

This project creates a reusable LXC container template that streamlines the setup of Ubuntu containers with Docker already installed and configured. It's designed to reduce the time needed to provision new containerized development and production environments.

## Features

- **Quick Container Setup**: Pre-configured Ubuntu LXC template with Docker installed
- **Ready-to-Use Docker Environment**: Docker is already included and configured in the template
- **Automation**: Python-based scripts to automate container initialization and Docker setup
- **Ubuntu-based**: Built on Ubuntu for stability and broad compatibility

## Requirements

- LXC (Linux Containers)
- Ubuntu system (or compatible Linux distribution)
- Python 3.x
- Docker (for use within containers)

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

## Project Structure

```
lxc-ubuntu-docker-template/
├── README.md                    # This file
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

## Troubleshooting

### LXC not found
Ensure LXC is installed: `sudo apt-get install lxc lxc-templates`

### Permission denied when running lxc commands
Most LXC commands require sudo privileges. Use `sudo` when necessary.

### Docker not available in container
Verify that Docker was successfully installed during template creation. You may need to restart the container or run additional setup steps.

---

**Last Updated**: June 2026
