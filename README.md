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
  - **Gotify** - A simple server for sending and receiving messages
  - **Watchtower** - A container-based solution for automating Docker container base image updates.

- **Network**
  - **Pi-Hole** - DNS and ad blocker

- **Backup**
  - **Duplicati** - Duplicati is a backup client that securely stores encrypted, incremental, compressed remote backups of local files on cloud storage services and remote file servers. 

- **HASS**
  - **Home Assistant** - Home Assistant is free and open-source software used for home automation 
  - **Mosquitto Broker** - MQTT acts like a messaging service that makes it easier for Home Assistant to send and receive messages from sensors and other devices


## Getting Started

### Stacks Persistent Data

Create an `/opt/stacks` folder or update the `STACK_DATA` variable in the `.env` file (see below).
```bash
sudo mkdir -p /opt/stacks
  ```

### Environment Variables

Rename `blank_env` to `.env` and update all variables according to your needs and setup. Note that the script will not run if `.env` is not detected.


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

To receive Watchtower notififications in Gotify:

1. Open Gotify web interface `http://<IPADDRESS>:2080`
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

Ref: https://pimylifeup.com/home-assistant-docker-compose/


```bash
sudo mkdir -p /opt/stacks/hass
```

#### Setting up Mosquitto 

```bash
sudo groupadd -g 1883 mosquitto
sudo useradd -u 1883 -g 1883 mosquitto
sudo mkdir -p /opt/stacks/hass/mosquitto/config
sudo nano /opt/stacks/hass/mosquitto/config/mosquitto.conf
```

Enter the following lines and save

```
persistence true
persistence_location /mosquitto/data/
log_dest file /mosquitto/log/mosquitto.log
listener 1883
listener 9001
allow_anonymous true
```

```bash
sudo chown -R mosquitto: /opt/stacks/hass/mosquitto
```

#### Deploy the HASS stack

```bash
smart-home hass up
  ```

#### Complete configuration

1. Access `http://<IPADDRESS>:8123` and create your account for Home Assistant
2. Click on Settings, "Devices & services" and  the + ADD INTEGRATION button 
3. Search for `mqtt` and select MQTT (it should be the first one found) 
4. Enter `localhost` as broker and click SUBMIT



## Important notes

- The initialization function assumes that you will use Pi-Hole and disables the default resolved service to avoid TCP port binding conflicts on TCP port 53.
- The Pi-Hole YAML configuration assumes you will keep using your current DHCP server (usually your modem/router).


## License

[MIT][license-url]

<!-- References -->

[license-url]: https://github.com/di-effe/smart-home/blob/master/LICENSE

