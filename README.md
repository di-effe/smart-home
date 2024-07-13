# Smart Home
#### Based on [**Denys Dovhan's Smart Home**](https://denysdovhan.com/smart-home)

## Hardware and OS
My home server is running on a Lenovo ThinkCentre M715Q Tiny, equipped with Ubuntu Server 22.04.

## Stacks

Stacks are groups of apps organized by a common trait and are physically defined by a directory containing a `compose.yml` file.

### Available Stacks

- **Essentials**
  - **Caddy Reverse proxy** - Caddy sports a flexible and powerful HTTP reverse proxy, on-line configuration API, and a robust, production-ready static file server, and serves all sites over HTTPS by default with automagic TLS certificates
  - **Portainer CE** - Manage containers via a user-friendly web UI
  - **Homarr** - Fancy dashboard for displaying home services
  - **Gotify** - A simple server for sending and receiving messages
  - **Watchtower** - A container-based solution for automating Docker container base image updates

- **Network**
  - **Pi-Hole** - DNS and ad blocker

- **Backup**
  - **Duplicati** - Duplicati is a backup client that securely stores encrypted, incremental, compressed remote backups of local files on cloud storage services and remote file servers

- **HASS**
  - **Home Assistant** - Home Assistant is free and open-source software used for home automation 
  - **Mosquitto Broker** - MQTT acts like a messaging service that makes it easier for Home Assistant to send and receive messages from sensors and other devices

- **Media**
  - **SABnzbd** - SABnzbd is an Open Source Binary Newsreader written in Python
  - **Prowlarr** - Prowlarr is an indexer manager/proxy
  - **Readarr** - Book Manager and Automation

## Getting Started


### DNS Records

Setup your local DNS records for

- Homarr (default value in .env is `dashboard.local`)
- Portainer (default value in .env is `portainer.local`)
- Gotify (default value in .env is `gotify.local`)
- Duplucati (default value in .env is `duplicati.local`)
- Pi-Hole (default value in .env is `pihole.local`)
- HomeAssistant (default value in .env is `hass.local`)
- SABnzbd (default value in .env is `sabnzbd.local`)
- Prowlarr (default value in .env is `prowlarr.local`)
- Readarr (default value in .env is `readarr.local`)

### Environment Variables

Rename `blank_env` to `.env` and update all variables according to your needs and setup. Note that the script will not run if `.env` is not detected.


### Stacks Persistent Data

Directory set as `STACK_DATA` variable (by default /opt/stacks) in the `.env` file will be created during the init (see below) 


### Init
- Add smart-home script to `PATH` and set `SMART_HOME_DIR`:
  ```bash
  echo 'export SMART_HOME_DIR="$HOME/smart-home"' >> ~/.bashrc
  echo 'export PATH="$PATH:$HOME/smart-home/bin"' >> ~/.bashrc
  source ~/.bashrc
  ```
- Initialize the home server

```bash
smart-home init
  ```


### Deploy essentials tools
```bash
smart-home essentials up
  ```

To receive Watchtower notififications in Gotify:

1. Open Gotify web interface `https://gotofy.local` (or whatever you set in .env as `CADDY_GOTIFY`)
2. click on APPS
3. Click on CREATE APPLICATION
4. Enter Watchtower as name, and click CREATE
5. Take note of the Token
6. Edit the `.env` file
7. Fill up `WATCHTOWER_GOTIFY_TOKEN` with the newly created token


```bash
smart-home essentials down && smart-home essentials up
```

### Deploy network tools
```bash
smart-home network up
  ```

### Deploy backup tools
```bash
smart-home backup up
  ```


### Deploy Home Assistant tools

```bash
smart-home hass up
  ```

#### Complete configuration

1. Access `http://<IPADDRESS>:8123` or `http://hass.local:8123`  and create your account for Home Assistant
2. Click on Settings, "Devices & services" and  the + ADD INTEGRATION button 
3. Search for `mqtt` and select MQTT (it should be the first one found) 
4. Enter `localhost` as broker and click SUBMIT


### Deploy media tools
```bash
smart-home media up
  ```


## Important notes

- The initialization function assumes that you will use Pi-Hole and disables the default resolved service to avoid TCP port binding conflicts on TCP port 53.
- The Pi-Hole YAML configuration assumes you will keep using your current DHCP server (usually your modem/router).


## License

[MIT][license-url]

<!-- References -->

[license-url]: https://github.com/di-effe/smart-home/blob/master/LICENSE

