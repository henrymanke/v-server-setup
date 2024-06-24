# V-Server Setup

<details>

<summary>Table of contents</summary>

1. [V-Server Setup](#v-server-setup)
   - [Create SSH Key Locally](#create-ssh-key-locally)
   - [Connect to Server via SSH](#connect-to-server-via-ssh)
   - [Store Pub-Key on Server](#store-pub-key-on-server)
   - [Connect to Server without Credentials](#connect-to-server-without-credentials)
   - [Disable Password-Logins](#disable-password-logins)
   - [Check if Password Authentication is Disabled](#check-if-password-authentication-is-disabled)
2. [Installing Webserver nginx](#installing-webserver-nginx)
   - [Create Alternative `index.html` for nginx](#create-alternative-indexhtml-for-nginx)
   - [nginx Setup](#nginx-setup)
4. [Add GitHub SSH Key on Server](#add-github-ssh-key-on-server)

</details>

## Create SSH Key Locally

```sh
ssh-keygen -t ed25519
```

## Connect to Server via SSH

```sh
ssh <user>@<hostname>
# Example:
ssh hmanke@188.245.34.78
```

## Store Pub-Key on Server

```sh
ssh-copy-id -i $HOME/.ssh/id_ed25519.pub <user>@<hostname>
# Example:
ssh-copy-id -i ~/.ssh/id_ed25519.pub hmanke@188.245.34.78
```

## Connect to Server without Credentials

- Tutorial:

  ```sh
  ssh -i ~/.ssh/id_ed25519.pub hmanke@188.245.34.78
  ```

- For me it also works with only:

  ```sh
  ssh hmanke@188.245.34.78
  ```

## Disable Password-Logins

Adjust `/etc/ssh/sshd_config` with:

```sh
sudo nano /etc/ssh/sshd_config
```

Uncomment this line and set it to `no`:

```plaintext
#PasswordAuthentication yes
        |
        V
PasswordAuthentication no
```

Save and exit the editor (nano).

Restart `sshd` with:

```sh
sudo systemctl restart ssh.service
```

## Check if Password Authentication is Disabled

```sh
ssh -o PubkeyAuthentication=no hmanke@188.245.34.78
```

Expected result:

```plaintext
hmanke@188.245.34.78: Permission denied (publickey).
```

## Installing Webserver nginx

1. Update system:

   ```sh
   sudo apt update
   ```

2. Install nginx:

   ```sh
   sudo apt install nginx -y
   ```

3. Check nginx status:

   ```sh
   systemctl status nginx
   ```

4. Visit hostname via browser: `http://188.245.34.78/`

## Create Alternative `index.html` for nginx

1. Create alternative HTML file:

   ```sh
   touch base-index.html
   ```

2. Make directory:

   ```sh
   sudo mkdir /var/www/alternatives/
   ```

3. Add HTML file:

   ```sh
   sudo touch /var/www/alternatives/base-index.html
   ```

4. Add content:

   ```sh
   sudo nano /var/www/alternatives/base-index.html
   ```

## nginx Setup

1. Duplicate default nginx config:

   ```sh
   cd /etc/nginx/sites-enabled/
   sudo cp default alternatives
   ```

2. Adjust alternatives:

   ```sh
   sudo nano alternatives
   ```

   ```nginx
   server {
           listen 8069 default_server;
           listen [::]:8069 default_server;
           
           root /var/www/alternatives;
           index base-index.html;

           server_name _;

           location / {
                   try_files $uri $uri/ =404;
           }
   }
   ```

3. Check configuration:

   ```sh
   sudo nginx -t
   ```

4. Restart nginx:

   ```sh
   sudo service nginx restart
   sudo systemctl restart nginx.service
   ```

5. Check browser: `http://188.245.34.78:8069/`

## Add GitHub SSH Key on Server

1. Configure Git on server:

   ```sh
   git config --global user.name "your-username"
   git config --global user.email "your.email@example.com"
   ```

2. Generate key on server:

   ```sh
   ssh-keygen -t ed25519 -C "your.email@example.com"
   ```

3. Ensure the SSH agent is running and has loaded the key:

   ```sh
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```

4. Display public key:

   ```sh
   cat ~/.ssh/id_ed25519.pub
   ```

5. Copy public key to [GitHub SSH Settings](https://github.com/settings/ssh/new)

6. Test connection to GitHub:

   ```sh
   ssh -T git@github.com
   ```