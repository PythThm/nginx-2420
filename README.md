This is a tutorial that would take you from a fresh Arch Linux server runnning on DigitalOcean to serving the demo documents included below

Start with updating your Operating System and download nginx

```
sudo pacman -Syu
sudo pacman -S nginx
```


Check the status of nginx.service and start and enable it

```
sudo systemctl status nginx.service
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
```


If there is any error, try redownloading nginx completely

```
sudo pacman -Rs nginx
```


We can now start with creating directories in root, make sure you are not in your home directory

```
sudo mkdir /web/html/nginx-2420
sudo mkdir /etc/nginx/sites-available
sudo mkdir /etc/nginx/sites-enabled
```


Create a file inside the sites-available directory called `example.conf` that contains server block:
Start with `sudo vim /etc/nginx/sites-available/example.conf`
```
/etc/nginx/sites-available/example.conf
server {
    # Copy your server block that is in your nginx.conf
    # nginx.conf content can be accessed through cat /etc/nginx/nginx.conf
    # change location's root path to /web/html/nginx-2420
}
```


Create a symbolic link using `ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/example.conf` to enable a site


Append `include sites-enabled/*;` to the end of the `http`block, change the location root path to `/web/html/nginx-2420`
Start with `sudo vim /etc/nginx/nginx.service`
```
/etc/nginx/nginx.conf
http {
    ...
    include sites-enabled/*;
}
```

Run `sudo nginx -t` to ensure there are no syntax errors


Restart Nginx using `sudo systemctl restart nginx`


Correct setup example as below:
`/etc/nginx/sites-available/example.conf`:
![Alt text](image-2.png)


`/etc/nginx/nginx.conf`:
![Alt text](image-3.png)


Now we put some content into the website document, start with making a new `.html` file and edit it
```
cd /web/html/nginx-2420
sudo touch index.html
sudo vim index.html
```
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2420</title>
    <style>
        * {
            color: #db4b4b;
            background: #16161e;
        }
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
            font-family: sans-serif;
        }
    </style>
</head>
<body>
    <h1>All your base are belong to us</h1>
</body>
</html>
```


Now run the server with your droplet ip
How the result should look like:
![Alt text](image-4.png)
