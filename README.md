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

- **Backup**
  - **Duplicati** - Duplicati is a backup client that securely stores encrypted, incremental, compressed remote backups of local files on cloud storage services and remote file servers. 

- **HASS**
  - **Home Assistant** - Home Assistant is free and open-source software used for home automation 
  - **HASS Configurator** - Configuration UI for Home Assistant
  - **Node-RED** - Node-RED is a flow-based, low-code development tool for visual programming 
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


#### Setting up Node-RED

```bash
sudo mkdir /opt/stacks/hass/nodered
sudo chown 1000:1000 /opt/stacks/hass/nodered
```

#### Setting up HASS Configurator

```bash
sudo mkdir /opt/stacks/hass/hass-config/
sudo mkdir /opt/stacks/hass/configurator-config
sudo nano /opt/stacks/hass/configurator-config/settings.conf
```

Enter the following lines and save

```
{
    "BASEPATH": "../hass-config"
}
```

```bash
sudo nano /opt/stacks/hass/hass-config/configuration.yaml
```

Enter the following lines, replacing `<IPADDRESS>` with your server IP or hostname, and save

```
default_config:

panel_iframe:
  configurator:
    title: Configurator
    icon: mdi:wrench
    url: http://<IPADDRESS>:3218/
    require_admin: true
  nodered:
    title: Node-Red
    icon: mdi:shuffle-variant
    url: http://<IPADDRESS>:1880/
    require_admin: true
```

#### Deploy the HASS stack

```bash
smart-home hass up
  ```

#### Complete configuration

1. Access `http://<IPADDRESS>:8123` and create your account for Home Assistant
2. Click on your profile name in the bottom-left corner
3. Click on the Security tab at the top of the page
4. Look for Long-lived access tokens and create a new one called Node-RED
5. Take note of the token
6. Back to Home Assistant main menu (on the left), click on Node-Red
7. Click the hamburger menu icon (Three lines) in the top-right corner
8. Click on Manage palette abd select the Install tab
9. Search for `node-red-contrib-home-assistant-websocket` and click Install
10. Click Install on the red button and then close that window
11. Back to the main Node-Red page, scroll down the cute colored buttons, look for `events:all` and drag it on the main empty space on the right
12. Double-click on it abd ib the new panel click on the `+` sign to add a new server
13. Enter your Home Assistant server address `http://<IPADDRESS>:8123`, the token generated earlier and click Add
14. With the server added, you must click the Deploy button in the top-right 
15. Back to Home Assistant main menu (on the left), click on Settings'
16. Navigate the settings menu and click the "Devices & services" and click the + ADD INTEGRATION button 
17. Search for `mqtt` and select MQTT (it should be the first one found) 
18. Enter `localhost` as broker and click SUBMIT


## Important notes

- The initialization function assumes that you will use Pi-hole and disables the default resolved service to avoid TCP port binding conflicts on TCP port 53.
- The Pi-hole YAML configuration assumes you will keep using your current DHCP server (usually your modem/router).


## License

[MIT][license-url]

<!-- References -->

[license-url]: https://github.com/di-effe/smart-home/blob/master/LICENSE

