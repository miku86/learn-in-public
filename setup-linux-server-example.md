# Linux Server

- Server: xxx
- URL: xxx
- Port: 2200
- User: miku86

## Installed software

`finger, apache2, libapache2-mod-wsgi, postgresql, git`

## Steps

- Add Lightsail Instance
- Add public static IP
- Add Port 2200 to firewall
- Use default SSH key to connect to server as `ubuntu`
- Updated all packages
- Change SSH port: `/etc/ssh/sshd_config`: `Port 2200` (https://www.ubuntu18.com/ubuntu-change-ssh-port/)
- Change local timezone to UTC: `sudo timedatectl set-timezone UTC` (https://askubuntu.com/questions/3375/how-to-change-time-zone-settings-from-the-command-line)
- Setup ufw for www, 123 and 2200
- Add user `miku86`
- Add miku86 to sudo: `sudo nano /etc/sudoers.d/miku86`
- Create an SSH key pair for miku86
- Move private into .ssh/authorized_keys of miku86
- Make .ssh 700 and authorized_keys 644
- Login to `miku86`
- Force key-based auth: `/etc/ssh/sshd_config`: `PasswordAuthentication no`
- Only `miku86` can login: `/etc/ssh/sshd_config`: `PermitRootLogin no` and `AllowUsers miku86` (https://askubuntu.com/questions/27559/how-do-i-disable-remote-ssh-login-as-root-from-a-server)
- Install apache, git, libapache2-mod-wsgi, python-dev
- Clone catalog app to `/var/www`
- Make `pg_config.sh` executable and run it to install all dependencies for my project
- Add `catalog.wsgi`: (https://flask.palletsprojects.com/en/1.1.x/deploying/mod_wsgi/#working-with-virtual-environments)
- Add virtualhost setup to `/etc/apache2/sites-available/catalog.conf` and enable it (https://httpd.apache.org/docs/2.4/vhosts/examples.html)
- Install postgresql
- Create database and user `catalog`, revoke public access
- Update the python app to use postgresql

## Summary

- [x] The SSH key submitted with the project can be used to log in as miku86 on the server.
- [x] You cannot log in as root remotely.
- [x] The miku86 user can run commands using sudo to inspect files that are readable only by root.
- [x] Only allow connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
- [x] Key-based SSH authentication is enforced.
- [x] All system packages have been updated to most recent versions.
- [x] SSH is hosted on non-default port.
- [x] The web server responds on port 80.
- [x] Database server has been configured to serve data.
- [x] Web server has been configured to serve the Catalog application as a WSGI app.
