# webhook-ci-cd

### 1. Install webhook 
`sudo apt install webhook`

### 2. Create a systemd unit to run webhook in the background.
    sudo nano /lib/systemd/system/gitwebhook.service

Paste the following:
    
```
[Unit]
Description=Automatically deploy upon push to github repository
After=syslog.target network.target

[Service]
Type=simple
User=YOUR_USERNAME_HERE
WorkingDirectory=/root/project
ExecStart=/usr/bin/webhook -hooks /root/webhooks/hooks.json -ip "0.0.0.0"

Restart=always

[Install]
WantedBy=multi-user.target
```

### 3. Enable the gitwebhook.service
```
sudo systemctl daemon-reload
sudo systemctl start gitwebhook
sudo systemctl enable gitwebhook
```

### 4. Open port 9000
`sudo ufw allow 9000`

### 5. Generate ssh-keys using `ssh-keygen` and append public key to your github repository as deploy key.
Do not use a passphrase while  generating ssh-keys. Keep it blank.

### 6. Create a webhook on your github repository 
Enter this as URL:
`https://127.0.0.1:9000/webhooks/github_repository`

* Replace 127.0.0.1 with external IP address of your server. 
* Replace `github_repository` with your github repository.
* Enter Secret Key same as on `hooks.json` file
* Set content type `application/json`
