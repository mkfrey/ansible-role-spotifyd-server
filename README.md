# ansible-role-spotifyd-server
Ansible role to install and configure spotifyd as a
system-wide service.

Currently should work on RedHat and Debian based distributions,
tested on Raspberry Pi with Fedora and Ubuntu. Please
feel free to submit a PR to add other distros as well. Just
creating a fitting file in `vars` should be enough.

## What this role does
- Install dependencies of spotifyd with alsa
- Compile and install spotifyd
- Create a system user to run spotifyd as
- Create paths and configuration files
- Create systemd service file

## What this role (currently) doesn't
- Support all configuration options provided by spotifyd
- Install spotifyd on a per user base
- Enable and start the created systemd service

## Variables
### spotifyd_username
Username for Spotify authenication.

Required.

### spotifyd_password
Password for Spotify authentication.

Required.

### spotifyd_install_directory
Installation root for cargo install.

Will be created if not present.

Note: If you set this to a home directory on a system with 
SELinux enabled, the systemd might not start the service.

Defaults to `/opt/spotifyd`.

### spotifyd_config_directory
Directory where the generated spotifyd.conf will be put.

Defaults to `/etc/spotifyd`

### spotifyd_cache_directory
Cache directory.

Will be created if not present.

Defaults to `/opt/spotifyd/cache`.

### spotifyd_compile_threads
Number of threads used for compilation by cargo install.

Note: When the compilation process gets terminated and
the error message indicates it was killed by `SIGKILL`,
chances are the OOM killer did it. Reducing the number
of threads may help, eg. on Raspberry Pi with 1GB using
only a single thread is recommended.

Defaults to `0` (automatic).

### spotifyd_daemon_user
The user that spotifyd will run as.

Will be created as system user if not present. User is
put in group `audio`.

Defaults to `spotifyd`.

### spotifyd_device_name
Name of the device shown in Spotify clients.

Defaults to `spotifyd`.

### spotifyd_device_type
The displayed device type in Spotify clients.

Possible values are `unknown`, `computer`, `tablet`, 
`smartphone`, `speaker`, `tv`, `avr` (Audio/Video Receiver),
`stb` (Set-Top Box) and `audiodongle`.

Defaults to `speaker`.

### spotifyd_bitrate
Audio bitrate.

Possible values are `96`, `160` and `320`.

Defaults to `160`.

### spotifyd_initial_volume
The initial volume spotifyd is started with.

Values from `0` to `100`.

Defaults to `50`.

### spotifyd_backend
Spotifyd audio backend.

Defaults to `alsa`. Other backends are not actively supported
by this role.

### spotifyd_volume_controller
Volume controller.

Possible values include `alsa`, `alsa_linear` and `softvol`.

Defaults to `softvol`.

### spotifyd_mixer
Alsa mixer used.

Defaults to `PCM`.

### spotifyd_device
Alsa audio device used.

Defaults to `default`.

### spotifyd_control.
Alsa control device used. Should most likely be the same value
as `spotifyd_device`.

Defaults to `default`.

## Minimal example
```
spotifyd_username: mySpotifyUserName
spotifyd_password: mySuperSecretSpotifyPassword # Using vault is recommended
```
