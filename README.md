# docker-snap-volume

[![GitHub Repo](https://img.shields.io/badge/GitHub-Repo-blue?logo=github)](https://github.com/BillRizer/docker-snap-volume)

## Overview

**docker-snap-volume** is a simple, command-line tool designed to create and restore snapshots of Docker volumes. It exports the contents of a specified Docker volume into a compressed `.tar` archive file for backup purposes and allows you to restore the volume from that archive later. This is particularly useful for scenarios where you need to quickly revert a volume to a previous state, such as during development, testing, or database management, without relying on slow data import processes.

Docker volumes are persistent storage mechanisms used by containers to store data independently of the container lifecycle. However, Docker's built-in tools don't provide a straightforward way to snapshot volumes efficiently. This script bridges that gap by leveraging Docker's container capabilities to mount and archive/restore volume data in a portable format.

Key features:
- **Fast Snapshots**: Create a `.tar` backup of a volume's entire contents.
- **Easy Restoration**: Restore from a `.tar` file directly to a volume, overwriting existing data if needed.
- **Lightweight**: No additional dependencies beyond Docker and standard Unix tools like `tar`.
- **Portable**: The `.tar` files can be stored, shared, or versioned easily (e.g., in Git or cloud storage).

This tool is ideal for developers, DevOps engineers, or anyone working with Docker-based applications that require frequent data resets or backups, such as databases (e.g., MySQL, PostgreSQL) or file-based services.

## Motivation

The project was born out of a real-world need during the development of a script to patch a large database. Traditional methods, like exporting and importing database dumps, were extremely time-consuming—often taking over 7 hours for large datasets. By creating a snapshot tool specifically for Docker volumes, we can now capture the volume's state in minutes and restore it just as quickly, streamlining workflows and reducing downtime in iterative development or testing environments.

## Prerequisites

- Docker installed and running on your system.
- Administrative privileges (e.g., `sudo` access) for installing the script and accessing volumes.
- Basic familiarity with command-line interfaces.

Note: This tool assumes the Docker volume exists and is not in use by running containers during snapshot creation or restoration to avoid data corruption. It's recommended to stop any dependent containers before operations.

## Installation

To install the script globally on your system for easy access:

1. Download the script using `curl`:

   ```
   sudo curl -SL https://raw.githubusercontent.com/BillRizer/docker-snap-volume/main/docker-snap-volume -o /usr/local/bin/docker-snap-volume
   ```

2. Make it executable:

   ```
   sudo chmod +x /usr/local/bin/docker-snap-volume
   ```

After installation, you can run `docker-snap-volume --help` to verify it's working and see usage instructions.

## Usage

The tool supports two main operations: creating a snapshot (`--create`) and restoring from a snapshot (`--restore`). Both require specifying the volume name (`-n`) and the path to the `.tar` file (`-f`).

### Creating a Snapshot

This command exports the contents of a Docker volume to a `.tar` file.

```
docker-snap-volume -n <volume-name> -f /absolute/path/to/volume-snapshot.tar --create
```

- `-n <volume-name>`: The name of the Docker volume (e.g., `myapp_db_data`). You can list available volumes with `docker volume ls`.
- `-f /absolute/path/to/volume-snapshot.tar`: The full path where the snapshot `.tar` file will be saved. Use an absolute path to avoid issues.

**Example**:
```
docker-snap-volume -n myapp_db_data -f /backups/db_snapshot_2025-10-16.tar --create
```

This will create a temporary Docker container to mount the volume, archive its contents using `tar`, and save it to the specified file. The process is efficient for most volumes but may take time for very large ones.

### Restoring a Snapshot

This command restores the contents from a `.tar` file back into a Docker volume, overwriting any existing data.

```
docker-snap-volume -n <volume-name> -f /absolute/path/to/volume-snapshot.tar --restore
```

- `-n <volume-name>`: The target Docker volume to restore into.
- `-f /absolute/path/to/volume-snapshot.tar`: The path to the existing `.tar` snapshot file.

**Example**:
```
docker-snap-volume -n myapp_db_data -f /backups/db_snapshot_2025-10-16.tar --restore
```

Similar to creation, this uses a temporary container to mount the volume and extract the `.tar` contents. Ensure the volume is not in use to prevent conflicts.

**Tips**:
- Always back up important data before restoring, as this operation is destructive.
- For automated workflows, integrate this into scripts or CI/CD pipelines.
- If dealing with very large volumes, consider compressing the `.tar` further (e.g., with `gzip`) outside of this tool.

## Contributing

Contributions are welcome! If you'd like to improve the script, add features (e.g., compression options, error handling, or support for volume drivers), or fix bugs:
- Fork the repository.
- Create a feature branch.
- Submit a pull request with a clear description of changes.

Please open issues for bug reports, feature requests, or questions. Let's make this tool even better!

## License

This project is open-source and available under the [MIT License](LICENSE). Feel free to use, modify, and distribute it as needed.

## About

Developed by [BillRizer](https://github.com/BillRizer). No additional website or topics provided yet—stay tuned for updates!

If you find this tool useful, star the repo or share it with others. For support, check the issues tab or contribute directly.
