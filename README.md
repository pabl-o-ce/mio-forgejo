# mio-forgejo

This repository contains the Docker Compose configuration for setting up a Forgejo instance with a PostgreSQL database and a Forgejo runner.

## Services

### Forgejo

- Image: `codeberg.org/forgejo/forgejo:8.0.3-rootless`
- Container name: `forgejo`
- Ports: 2222:2222
- Volumes:
  - forgejo data
  - forgejo config
  - timezone and localtime (read-only)

### Forgejo Database

- Image: `postgres:14`
- Volumes: PostgreSQL data

### Forgejo Runner

- Image: `gitea/act_runner:latest`
- Volumes: Docker socket
- Extra hosts:
  - git-domain:IP
  - registry-domain>:IP

## Networks

- forgejo (internal)
- middle-earth (external)

## Volumes

- forgejo-postgres
- forgejo-data (bind mount to `/mnt/vol1/forgejo/data`)
- forgejo-config (bind mount to `/mnt/vol1/gitea/config`)

## Setup

1. Ensure Docker and Docker Compose are installed on your system.
2. Clone this repository:
   ```
   git clone https://github.com/yourusername/mio-forgejo.git
   cd mio-forgejo
   ```
3. Create a `.env` file in the same directory as the `docker-compose.yml` file with the necessary environment variables.
4. Start the services:
   ```
   docker-compose up -d
   ```

## Configuration

The services are configured using environment variables. Make sure to set the following in your `.env` file:

- Database credentials for the Forgejo and PostgreSQL services
- Any additional configuration options for Forgejo
- Runner registration token and other necessary settings

## Accessing Forgejo

Once the services are up and running, you can access Forgejo at `http://localhost:3000` (assuming you've mapped port 3000 in your actual setup).

SSH access for Git operations is available on port 2222.

## Maintenance

To update the services:

1. Pull the latest images:
   ```
   docker-compose pull
   ```
2. Restart the services:
   ```
   docker-compose up -d
   ```

For backing up data, ensure you have a strategy in place for the PostgreSQL data volume and the Forgejo data volume.

## Contributing

Please read CONTRIBUTING.md for details on our code of conduct, and the process for submitting pull requests.

## License

This project is licensed under the [MIT License](LICENSE).
