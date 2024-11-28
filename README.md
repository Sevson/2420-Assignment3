# 2420-Assignment3 Part 1
# Setup Instructions

## Nginx Configuration

1. Save the Nginx configuration file:

    ```bash
    sudo nano /etc/nginx/nginx.conf
    ```
    (Add your Nginx configuration content and save the file.)

2. Create a symbolic link for the generated HTML directory:

    ```bash
    sudo ln -s /var/lib/webgen/HTML /usr/share/nginx/html
    ```

3. Reload Nginx to apply the new configuration:

    ```bash
    sudo systemctl reload nginx
    ```

## Systemd Service

1. Save the service file:

    ```bash
    sudo nano /etc/systemd/system/generate_index.service
    ```
    (Add your service configuration content and save the file.)

2. Reload `systemd` to recognize the new service:

    ```bash
    sudo systemctl daemon-reload
    ```

## Systemd Timer

1. Save the timer file:

    ```bash
    sudo nano /etc/systemd/system/generate_index.timer
    ```
    (Add your timer configuration content and save the file.)

2. Enable and start the timer:

    ```bash
    sudo systemctl enable generate_index.timer
    sudo systemctl start generate_index.timer
    ```
