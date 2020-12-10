# Setup local reverse proxy

## MacOS

To allow this to work on your local machine, follow these steps:

1. Install [nginx](https://www.nginx.com/) on your machine. For OSX users, you can run `brew install nginx`. If you
   don't have [homebrew](https://brew.sh/), you should install it.

2. After install, run: `sudo nginx`

3. Update the nginx configuration file in `/usr/local/etc/nginx/nginx.conf` with the following values in the `http`
   module.

```
server {
  listen                  80;
  listen                  443 ssl;
  ssl_certificate         /etc/ssl/certs/myssl.crt;
  ssl_certificate_key     /etc/ssl/private/myssl.key;
  server_name             globalLottery;

  # Backend Service
  location /local {
      proxy_pass  http://localhost:8000;
  }
  
  # React frontend
  location / {
      proxy_pass  http://localhost:3000/;
  }
}
```

3. Run `sudo nginx -s stop` and `sudo nginx` to update nginx with the new configurations.

4. Update your hosts file in `/etc/hosts` file with the following values.

```
127.0.0.1	    local.globallottery.com
```

## Linux (Ubuntu or Ubuntu-based distro)

With Ubuntu, the setup is very similar to MacOS, but ever so slightly different.

1. Install [nginx](https://www.nginx.com/) on your machine using `sudo apt install nginx`.

2. Create a new file for the reverse proxy at `/etc/nginx/sites-available/reverse-proxy.conf` with the contents
   from the MacOS section (step 2) above.

3. Create a symbolic link to the file in `sites-enabled`
   using `sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf`

4. Run `sudo nginx -s stop` and `sudo nginx` to update nginx with the new configurations.

5. Update your hosts file in `/etc/hosts` file with the following values from the MacOS section above (step 4).
