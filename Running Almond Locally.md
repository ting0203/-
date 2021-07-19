# Running Almond Locally

# Hardware & OS

This guide should work on any modern Linux OS, including recent versions of Ubuntu, Fedora, and Raspberry OS (using PulseAudio as the sound system). Almond should work with good performance on a Raspberry Pi 3 or 4, if you plug a USB microphone and external speakers.

> You can help us test additional Linux distributions and HW configurations. Please reach out to our [community forum](https://community.almond.stanford.edu/) or edit this page to add any distro specific notes.

**For development only**, you can also run Almond Server on Mac OS or Windows. You can use docker, or use a native installation. If you use Windows, you can use Windows Subsystem for Linux, or run the application natively. **Sound input and output is not available on Mac OS or Windows.**



# The Software

The local version of Almond is called "Almond Server". You can download it from [Docker Hub](https://hub.docker.com/r/stanfordoval/almond-server) or from the [source code repository](https://github.com/stanford-oval/almond-server).

## Container Installation

You need to use [podman](https://podman.io/) as your container runtime to ensure proper permissions to access PulseAudio from inside the container.

```bash
podman run --name almond -p 3000:3000 \
    -v /dev/shm:/dev/shm \
    -v $XDG_RUNTIME_DIR/pulse:/run/pulse \
    stanfordoval/almond-server:latest
```

After running the first time you can start and stop Almond with:

```bash
podman start almond
podman stop almond
```

**Note**: do not run the above commands as root, as it will lead to permission problems.

**Note**: on Fedora, you must disable SELinux (`sudo setenforce 0`) before running the above command.

**Note**: the above commands runs the latest version of Almond. Replace "latest" with a version tag (e.g. `v1.8.0`) to access an older version of Almond. See the [Release Notes](https://wiki.almond.stanford.edu/release-planning) for the features available in each version.

## Container Installation (Without Sound)

If you cannot use podman (e.g. on Mac OS or Windows), you can instead run Almond inside a container without sound support. You will be able to access Almond from the web interface instead. To do so, use the following command:

```bash
docker run --name almond -p 3000:3000 stanfordoval/almond-server:latest-portable
```

## Development Installation

To develop Almond, or for custom setups, you can install Almond from sources. Following the instructions in the [almond-server git repository](https://github.com/stanford-oval/almond-server) to do so.

# Accessing Almond

## First-time Configuration

The first time you run Almond, you use your browser to complete the configuration. Connect to localhost on port 3000 and follow the instructions there to finish installation.
You can later change any settings from the Configuration page, accessible from the navigation bar in the Almond web UI.

## In voice

If Almond is running, you can wake it up with the wake-word "computer". You will hear a "ding" sound when Almond recognizes the wake word, and you can issue your command in voice afterwards.

## From the web

You can use your browser and connect to localhost on port 3000 to access the web UI. Click the "Conversation" tab to access the UI to type commands, or click the "Skills" tab to configure new skills to use in text or voice.