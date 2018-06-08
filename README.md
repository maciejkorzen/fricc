# fricc

FRIendly Connection Chooser

I've created this program because I needed a way to organize my SSH, Rdesktop and VNC connections.
This program also handles SSH port redirections. If You use port redirection often, then this will be big win for You. Define it once and never worry about it again. It will be used always in future.

Following kinds of connections are supported:
- SSH
	- SFTP
	- SSHFS
- Rdesktop (MS RDP client)
	- Rdesktop in terminal zero (system console) mode
- [tsvnc](http://www.karlrunge.com/x11vnc/ssvnc.html)
- VNC
- Mosh

# Screenshots

...

# Requirements

- `dialog`
- `freerdp` or `rdesktop`
- Package providing `vncviewer` binary. For example `tigervnc` package on CentOS 7.

# Quick start
- Copy files from `bin` directory to `/usr/local/bin` or some other directory included in `PATH`.
- If you want to use sshfs, then create `/mnt/sshfs` directory and give your user write access to it. sshfs will be mounted in subdirectories of `/mnt/sshfs`.
- `mkdir -p ${HOME}/.fricc/{rdesktop,redirectports,ssh,tsvnc,vnc,vnc-keys}`
- Install required packages.
- Create first SSH connection definition file, just for testing:
>     cat > .fricc/ssh/localhost << EOF
>     name localhost-test
>     address 127.0.0.1
>     desc localhost test
>     EOF
- Run `fricc`.
- Select `SSH` from menu and then select connection entry.

That's all.
Keep reading to learn how to write connections' definitions.

# After quick start
In `fricc` file You can adjust sections inside `<customization>...</customization>` tags according to your likes.

Cache files are stored in `${HOME}/tmp/fricc`. You can delete them. Cache will be generated when missing or when configuration files are modified.

In `templates` directory you will find templates for configuration files with all available keywords specified.

In `home-dot-fricc-example` you will find examples of connection definitions. Examples are almost random, don't assume that they are logic.

# Connection definition file syntax description

All connections' definitions are stored in `~/.fricc` in subdirectories:

- `rdesktop` - used for Remote Desktop connections
- `ssh` - used for SSH, sshfs and sftp connections
- `tsvnc` - used for tsvnc connections
- `vnc` - used for VNC connections

Additionally there are following directories:

- `redirectports` - definitions of ports to redirect. Referenced in `ssh` connections. If `ssh` connection references port redirection file, and that file does not exist, `fricc` won't start (error message will be displayed).
- `vnc-keys` - keys (passwords) for VNC connections.

## SSH connection definition

Following keywords are supported:

| Keyword | Optional? | Default value | Description |
|-|-|-|-|
| `port` | Yes | 22 | |
| `address`  | Required unless `name` is provided  |  If empty, then `name` is used as it's value. | DNS name or IP address. |
| `name` | Required unless `address` is provided. | If empty, then `address` is used as it's value. | |
| `login` | Yes | If it's missing, then login from operating system is used. | |
| `desc` | Yes. | If empty then `desc` is composed automatically from `login`, `address` and `port`. | Connection description. |
| `dir` | Yes. | `/` | Used by `sshfs`. |
| `redirectports` | Yes. | None. | Name of file in `redirectports directory`. File must exist. |
| `args` | Yes. | None. | Additional arguments to `ssh` command. |
