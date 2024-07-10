# Smart Home
#### Based on [**Denys Dovhan's Smart Home**](https://denysdovhan.com/smart-home)

## Hardware and OS
My home server is running on a Lenovo ThinkCentre M715Q Tiny, equipped with Ubuntu Server 22.04.

## Stacks

Stacks are groups of apps organized by a common trait and are physically defined by a directory containing a `compose.yml` file.

### Available Stacks

- **Essentials**
  - **Portainer CE** - Manage containers via a user-friendly web UI
  - **Homarr** - Fancy dashboard for displaying home services
  - **Watchtower** - Automate Docker container base image updates

- **Network**
  - **Pi-hole** - DNS and ad blocker


## Getting Started

### Stacks Persistent Data

Create an `/opt/stacks` folder or update the `STACK_DATA` variable in the `.env` file (see below).

### Environment Variables

Rename `blank.env` to `.env` and update all variables according to your needs and setup. Note that the script will not run if `.env` is not detected.


### Init
- Connect via ssh on your server 
- Clone `smart-home` repository to home folder or restore from backup
- Add smart-home script to `PATH` and set `SMART_HOME_DIR`:
  ```bash
  echo 'export SMART_HOME_DIR="$HOME/smart-home"' >> ~/.bashrc
  echo 'export PATH="$PATH:$HOME/smart-home/bin"' >> ~/.bashrc
  source ~/.bashrc
  ```

### Deploy essentials tools
```bash
smart-home essentials up
  ```

### Deploy network tools
```bash
smart-home network up
  ```


## Important notes

- The initialization function assumes that you will use Pi-hole and disables the default resolved service to avoid TCP port binding conflicts on TCP port 53.
- The Pi-hole YAML configuration assumes you will keep using your current DHCP server (usually your modem/router).


## License

[MIT][license-url]

<!-- References -->

[license-url]: https://github.com/di-effe/smart-home/blob/master/LICENSE
