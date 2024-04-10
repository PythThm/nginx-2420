1. Update your system with `sudo pacman -Syu`
2. Download the `hello-server` file
3. Instead of logging in with ssh, login with `sftp`
4. Go to your bin directory with `cd /home/<username>/bin`
5. Put the downloaded file into the bin directory with `put hello-world`, change the path if you have to
6. Exit out of sftp and login regularly with ssh
7. Create a new service called `hello-server.service` with `sudo vim /etc/systemd/system/hello-server.service`
8. Copy and paste the following code into the file:
```
[Unit]
Description=hello-server start
After=network.target

[Service]
Type=Simple
ExecStart=/home/<username>/bin/hello-server

[Install]
WantedBy=multi-user.target
```

9. with `sudo vim /etc/nginx/sites-available/example.conf` to modify existing script
10. Under server block, add the following code into the file:
```
    location /api {
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /hey {
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /echo {
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
```
11. Make sure the file have execution permission with `sudo chmod +x sites-available`
12. Download ufw with `sudo pacman -S ufw`
13. Enable and start the service with `sudo systemctl enable --now ufw.service`
14. Allow http and ssh connections with:
`sudo ufw allow http`
`sudo ufw allow ssh`
15. Enable the firewall with `sudo ufw enable`
16. Reload systemd manager configurations with `sudo systemctl daemon-reload`
17. Reload nginx.service with `sudo systemctl reload nginx.servce`
18. Enable hello-server.service with `sudo systemctl enable hello-server.service`
19. Start hello-server.service with `sudo systemctl start hello-server.service`

TroubleShooting:
Replace the IP address below with your own, there commands should be run from your host machine, not from the server
`curl http://64.23.162.170/hey`
This should return `hey message`

`curl -X POST -H "Content-Type: application/json" -d '{"message": "Hello from your server"}' http://64.23.162.170/echo`
This should return `{"message": "Hello from your server"}`

If the curl command is not working properly, please use Postman for advanced testing
Replace the IP address with your own
