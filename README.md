# Smart Home
#### Based upon [**Denys Dovhan Smart Home**](https://denysdovhan.com/smart-home)

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
- Fill secret credentials in `$HOME/smart-home/.env` file. Use `smart-home password` to generate new passwords.
- Create symlinks between `$HOME/smart-home/.env` and each stack `.env` file
- Run `smart-home support start`. This command will pull down images and start the main containers such as Homarr and Portainer.



## License

[MIT][license-url] 

<!-- References -->

[license-url]: https://github.com/di-effe/smart-home/blob/master/LICENSE


