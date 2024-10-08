# Keycloak using Docker Compose

This repo is a modified version (exclude bundled traefik) of the original [repo](https://github.com/heyValdemar/keycloak-traefik-letsencrypt-docker-compose) of [Vladimir Mikhalev](https://www.docker.com/captains/vladimir-mikhalev/). See there for further informations.

❗ Copy `.env.example` to `.env` and change voriables to meet your requirements.

💡 Note that the `.env` file should be in the same directory as `keycloak-postgres-docker-compose.yml`.

Create networks for your services before deploying the configuration using the commands:

`docker network create keycloak-network`

Change the traefic network name (proxy) in `keycloak-postgres-docker-compose.yml` according to your needs.

Deploy Keycloak using Docker Compose:

`docker compose -f keycloak-traefik-letsencrypt-docker-compose.yml -p keycloak up -d`

# Backups

The `backups` container in the configuration is responsible for the following:

1. **Database Backup**: Creates compressed backups of the PostgreSQL database using pg_dump.
Customizable backup path, filename pattern, and schedule through variables like `POSTGRES_BACKUPS_PATH`, `POSTGRES_BACKUP_NAME`, and `BACKUP_INTERVAL`.

2. **Backup Pruning**: Periodically removes backups exceeding a specified age to manage storage. Customizable pruning schedule and age threshold with `POSTGRES_BACKUP_PRUNE_DAYS` and `DATA_BACKUP_PRUNE_DAYS`.

By utilizing this container, consistent and automated backups of the essential components of your instance are ensured. Moreover, efficient management of backup storage and tailored backup routines can be achieved through easy and flexible configuration using environment variables.

# keycloak-restore-database.sh Description

This script facilitates the restoration of a database backup:

1. **Identify Containers**: It first identifies the service and backups containers by name, finding the appropriate container IDs.

2. **List Backups**: Displays all available database backups located at the specified backup path.

3. **Select Backup**: Prompts the user to copy and paste the desired backup name from the list to restore the database.

4. **Stop Service**: Temporarily stops the service to ensure data consistency during restoration.

5. **Restore Database**: Executes a sequence of commands to drop the current database, create a new one, and restore it from the selected compressed backup file.

6. **Start Service**: Restarts the service after the restoration is completed.

To make the `keycloak-restore-database.shh` script executable, run the following command:

`chmod +x keycloak-restore-database.sh`

Usage of this script ensures a controlled and guided process to restore the database from an existing backup.

# Author

Original by Vladimir Mikhalev, the [Docker Captain](https://www.docker.com/captains/vladimir-mikhalev/).

Modifications by Henry Jobst
