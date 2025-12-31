# Solar Assistant Docker Image Builder

## What is this?

This repository contains a `Dockerfile` specifically designed to download the official Solar Assistant disk image (for Raspberry Pi 64-bit), extract its filesystem, and package it into a minimal Docker container.

This allows you to run the Solar Assistant software within a Docker environment.

## How to Use

1.  **Prerequisites:** Make sure you have Docker installed and running.
2.  **Build the Docker Image:** Clone this repository and run the build command from within the directory:

    ```bash
    docker build -t your-repo/solar-assistant:latest .
    ```

    *   This uses the default download URL for the `rpi64` architecture specified in the `Dockerfile`.
    *   *(Optional)* You can override the download URL using `--build-arg IMG_PATH=<url>`.
    *   *(Optional)* If Solar Assistant changes its partitioning and the main filesystem isn't partition 2, use `--build-arg PARTITION_NUM=<number>`.

3.  **Run the Container:** Once the build finishes, you can run the Solar Assistant container:

    ```bash
    # Example: Expose web UI and attach serial device
    docker run -d \
      --name solar-assistant \
      -p 80:80 \
      --device=/dev/ttyUSB0 \
      --restart=unless-stopped \
      theblazehen/solar-assistant:latest
    ```
    *   Adjust port mappings (`-p`) and device passthrough (`--device`) according to your setup and the hardware Solar Assistant needs to access.
    *   The container starts using a `systemctl` replacement script, attempting to launch the Solar Assistant services.

## Important Notes

*   **Not a Full VM:** This runs the Solar Assistant filesystem in a container, not a full virtual machine. It uses your host's Linux kernel.
*   **Hardware Access:** You *must* pass through the necessary hardware devices (like USB serial converters for inverters/batteries) using `--device` flags for Solar Assistant to function correctly.
*   **`systemctl` Replacement:** The included `systemctl` replacement mimics systemd but isn't identical. Most standard Solar Assistant services should work, but complex service interactions might behave differently.

## License

This project is licensed under the GNU Affero General Public License v3.0 (AGPL-3.0).

See the [LICENSE](LICENSE) file for details.
