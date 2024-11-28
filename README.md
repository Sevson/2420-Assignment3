# 2420-Assignment3 Part 1

## Intro

The purpose of this repository is to create a script that runs daily at 5 AM using a systemd timer. This script generates a static HTML file with system information, which is served by an Nginx web server. Additionally, security is provided through a firewall setup using UFW.

---

## Getting Started

To set up this system, the following requirements must be met:
- **Root access** to necessary scripts and files.
- **Nginx** and **UFW** packages installed.

---

## Setting Up

### 1. Set up a new user
Create a new system user with no login shell and a home directory in `/var/lib/webgen`:
```bash
sudo useradd -r -d /var/lib/webgen -s /usr/sbin/nologin webgen
```
This will create the user webgen with the specified directory.

### 2. Create home directories

Create the required directories for the user:
```bash
sudo mkdir -p /var/lib/webgen/bin /var/lib/webgen/HTML
```
### 3. Clone index_generate script

Copy the index_generate script to /var/lib/webgen/bin:
```bash
sudo cp 2420-Assignment3/assets/index_generate /var/lib/webgen/bin
```

Setting up .timer and .service for generate_index
1. Copy the systemd service and timer files

Copy the provided generate_index.timer and generate_index.service files to /etc/systemd/system:

sudo cp ~/assignment3/generate_index.{service,timer} /etc/systemd/system

2. Reload systemd

After copying the files, reload systemd to recognize the new services and timer:

sudo systemctl daemon-reload

3. Enable and start the timer

Enable and start the systemd timer:

sudo systemctl enable generate_index.timer
sudo systemctl start generate_index.timer

4. Verify the service and timer are running successfully
Check when the timer will run next:

systemctl list-timers --all | grep generate_index.timer

Check if the service ran successfully:

sudo systemctl status generate_index.service

View logs for the service:

journalctl -u generate_index.service

View logs for the timer:

journalctl -u generate_index.timer

Setting Up Nginx
1. Create directories for server blocks

Create the directories required for the server block configuration:

sudo mkdir /etc/nginx/sites-available /etc/nginx/sites-enabled

2. Copy the Nginx configuration file

Copy the provided default.conf file to /etc/nginx/sites-available:

sudo cp ~/assignment3/default.conf /etc/nginx/sites-available

3. Create a symbolic link

Create a symbolic link to enable the server block:

ln -s /etc/nginx/sites-available/default.conf /etc/nginx/sites-enabled/default.conf

4. Start and enable Nginx

Start and enable the Nginx service:

sudo systemctl start nginx
sudo systemctl enable nginx

5. Check the status and test Nginx configuration
Check the Nginx service status:

sudo systemctl status nginx

Check for any errors in the Nginx configuration:

sudo nginx -t

Apply any changes to the Nginx configuration:

sudo systemctl reload nginx

Note
Why is it important to use a separate server block file instead of modifying the main nginx.conf file?

Using a separate server block keeps configurations organized and easy to manage. It reduces errors, simplifies troubleshooting, and makes backups and scaling easier compared to editing the main nginx.conf file directly.
Setting Up UFW (Uncomplicated Firewall)
1. Enable and start the UFW service

Enable and start the UFW firewall:

sudo systemctl enable --now ufw.service

2. Allow SSH and limit the rate to prevent brute-force attacks

Allow SSH connections and limit the rate:

sudo ufw allow ssh
sudo ufw limit ssh

3. Allow HTTP connections

Allow HTTP (port 80) traffic:

sudo ufw allow http

4. Enable the firewall

Enable the firewall:

sudo ufw enable

5. Check the status of UFW

Check the status of the firewall to confirm it's working:

sudo ufw status verbose

![ufw](https://github.com/user-attachments/assets/2b2ff220-0f63-4cfa-b47e-e67a9cc08c12)
![webserver](https://github.com/user-attachments/assets/d1922bb4-d31d-446a-a1ec-d5dcb90c4942)
