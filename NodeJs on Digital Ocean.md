#1 Droplet Creation
    - Select blank installation (meaning not a One-Click app)
    - Add new SSH (Optional but make life easier)
    - Change droplet's name (Optional)
    - Test Login with Putty
    - Done
#2 Create non root user
    - Connect to droplet using Putty (root@)
    - Add new user shoudou@
    - Add sudo priviledge to shoudou@
    - Switch to shoudou@ 
    - Create ~/.ssh folder and authorized_keys file 
    - Add SSH public key to authorized_keys
    - Restart SSH service
    - Test & fix
    - Done
#3 Add Github SSH key
    - Generate SSH key pair while in shoudou@
    - Add SSH id to ids
    - Copy&Paste public key to Github SSH settings
    - Restart SSH service from shoudou@
    - Test by cloning an app from Github
    - Install and run the app
    - Done
    - NB: You can use WinSCP to drag public key from the droplet and drop to your local machine
#4 Install and Set Nginx as Reserse Proxy Server
    - Install Nginx
    - Test Installation
    - Set up Nginx as Reverse Proxy Server
    - Test & fix
    - Done
#5 Add a domain name to your droplet
    - Domain name and setup at the registrar
    - Domain name and setup at Digital Ocean
    - Test & fix
    - Done
#6 Redirect requests to your app
    - Concepts
    - Setups
#7 Upgrade your app
    - Tools and Scenari
    - More tests
    