# Smart Home
### Based upon [**Denys Dovhan Smart Home**](https://denysdovhan.com/smart-home)

This is my personal smart-home configuration, awakening my home with services and tools. I hope this will help you inspire on the way to built your own smart home.

## Stacks

Stacks are logical app groupings and are phisically defined by a directory containing a `compose.yml` file.

## Getting Started

- Backup everything before performing any action.
  - Backup `smart-home` folder `tar czvf smart-home.tar.gz smart-home`
  - Copy archives locally via `scp`
- Install Ubuntu Server
- Connect via ssh 
- Clone `smart-home` repository to home folder or restore from backups
- Add smart-home binaries to `PATH` and set `SMART_HOME_DIR`:
  ```bash
  echo 'export SMART_HOME_DIR="$HOME/smart-home"' >> ~/.bashrc
  echo 'export PATH="$PATH:$HOME/smart-home/bin"' >> ~/.bashrc
  source ~/.bashrc
  ```
- Init smart-home via `smart-home init`.
- Fill secret credentials in `.env` file. Use `smart-home password` to generate new passwords.
- Go to Cockpit dashboard (`https://<ip>:`) and set hostname, mount external storage. Reboot after changes.
- Spin up containers via `smart-home start`. This command will pull down images and star up containers.


## License

[MIT][license-url] 

<!-- References -->

[license-url]: https://github.com/di-effe/smart-home/blob/master/LICENSE


