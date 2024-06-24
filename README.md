v-server-setup

Create ssh key local
`ssh-keygen -t ed25519 `

Connect to server via SSH
`ssh <user>@<hostname>`
ssh hmanke@188.245.34.78

Store Pub-Key on Server
`ssh-copy-id -i $HOME/.ssh/id_ed25519.pub <user>@<hostname>`
ssh-copy-id -i ~/.ssh/id_ed25519.pub hmanke@188.245.34.78

Connect to Server without Credentials
Tutorial: `ssh -i ~/.ssh/id_ed25519.pub hmanke@188.245.34.78`
For me it also works with only: `ssh hmanke@188.245.34.78`

Disable Password-Logins
adjust /etc/ssh/sshd_config with `sudo nano /etc/ssh/sshd_config`

Uncomment this line and set it to yes
`#PasswordAuthentication yes ` -> `PasswordAuthentication no`

Save and Exit editor (nano)

restart sshd with `sudo systemctl restart ssh.service`

Check if Password Auth is disabeld
ssh -o PubkeyAuthentication=no hmanke@188.245.34.78

-> hmanke@188.245.34.78: Permission denied (publickey).



Installing Webserver nginx


Update system with `sudo apt update`
Install nginx with `sudo apt install nginx -y`
Check nginx status `systemctl status nginx`

Visit hostname via browser http://188.245.34.78/


Create alternative index.html for nginx

create alternative HTML file
`base-index.html`

Make directory
sudo mkdir /var/www/alternatives/

Add html file
sudo touch /var/www/alternatives/base-index.html

Add content
sudo nano /var/www/alternatives/base-index.html

nginx setup

Duplicate default nginx config
cd /etc/nginx/sites-enabled/
sudo cp default alternatives

adjust alternatives
sudo nano alternatives

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


check configuration with 
sudo nginx -t

restart nginx
sudo service nginx restart
sudo systemctl restart nginx.service

check browser
http://188.245.34.78:8069/



Add github SSH on Server

Configure Git on server
git config --global user.name "your-username"
git config --global user.email "your.email@example.com"

generate Key on server
ssh-keygen -t ed25519 -C "your.email@example.com"

 <!-- ssh-keygen -t ed25519 -->

To ensure that the SSH agent is running and has loaded the key, execute these commands:
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

cat ~/.ssh/id_ed25519.pub

copy public key to https://github.com/settings/ssh/new

test connection to github
ssh -T git@github.com





