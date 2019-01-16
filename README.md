# Deploying a NodeJS App to Digital Ocean

In this article, I am trying to write down my frustrations hoping to not feel them again when I will try to get a NodeJS app up and running on Digital Ocean Droplets with all the necessary tools to make it a "production grade app". Let me specify that the term "production grade" is relative to what I was trying to do with my application. So, Let's not put ourselves in the shoes of the big giants of the internet.

In the past, I have always used Heroku for hosting my NodeJS applications. I truly like Heroku because it is so easy to work with. With my Github Student Pack account, I got to create more applications than before. I never struggle to deploy or manage any of my apps. At least, until I tried using NodeJS child processes to run multiple instances of my application. I was very excited when I saw my test app running on more than one thread. No problem was reported until tried a much bigger application. Bangg!!!
```
Error R14 (Memory quota exceeded)
```

In my readings, I learned that [I should not try to run multiple processes base on the calculation of physical number of CPUs](https://devcenter.heroku.com/articles/node-memory-use). I figured out what was going on and I fixed the issue. I learned how Heroku calculates the actual number of CPUs based on the memory of your dyno using a environment variable called 
```
WEB_CONCURRENCY 
```
That is where I understood the difference between a [PaaS](https://en.wikipedia.org/wiki/Platform_as_a_service) and a [IaaS](https://en.wikipedia.org/wiki/Infrastructure_as_a_service). I never use a IaaS before. To satisfy my curiosity, I started exploring the circomstences in which each service might be the best choice to fullfil my needs. I came accross Digital Ocean multiple times. So, I decided to look into it. I used my Github Student Pack account again to get a $50 coupon when I signed up. Everything went smoothly at the begining. I learned few Ubuntu, SSH, and SFTP commands then I started exploring how I can benefit from Digital Ocean. I mostly work with NodeJS. So, I decided to deploy a "production ready" NodeJS appication on Digital Ocean. The struggle started at that very moment. 

My first few attempts were faillures and frustrations because I was just copying commands from the different channels on Digital Ocean and pasting on the browser console. I did not really (try to) understand them though. I just wanted to see a working hello world NodeJS application. Five hours after I started my "hello world project", I got my NodeJS application to run locally on the server. I was relieved to see the hello world on the console. I let everything settle down for about 3 hours before I restarted the project again but with the objective of understanding the overall process. 

The following are the steps I took to get my application to run. 

## Disclamer

There are many ways of deploying a NodeJS application to Digital Ocean. The path I took is in no mean the best way of doing it. As a I said in the section above, I am just writing down the frustations I felt when I first tried to deploy my NodeJS appliation to Digital Ocean. The article is not intended also to make a recommendation of any service provider. You can choose whatever service that fits your needs and budget.

## How I Wanted My NodeJS App to Be?

NodeJS application development is so flexible that no one can claim to have the best application structure. At least, I beleive so. But many developers seem to agree with some features and pratices that a good NodeJS Production application should have. We are not going to argue about that today. 

These are the important features/behaviors I wanted my NodeJS app to have. The app should be able to: 
  - use MongoDB for persistancy
  - send emails
  - schedule future tasks
  - deleguate some tasks to background workers
  - restart automatically if the server restarts
  - run on multiple unicorn workers depending on the number of CPUs/memory available 
  - run behind a reverse proxy server
  - attempt recovery if a process dies unespectidly
  - more ...

I am going to stick only on putting my application behind a reverse proxy server in this article. if you want to see a sample program that implements some of the behaviors listed above, visit my [Github](https://github.com/shoudou) profile.

## Let's Get Started

### Creating a Brand new Droplet

##### My Struggle 

Creating a server (droplet) on Digital Ocean seems to be easy. It is actually very easy because the interface is friendly and the whole process takes only a few steps. So, why did I struggle to create a simple droplet? Humm! In fact, I created my first droplets successfully. But, I wanted to understand what was going on "behind the scene", what flexibility margin the plateform gives me, and how I can dictate my own rules for most of the features instead of letting Digital Ocean setting up everything for me. Let's be Engineers! 

I worked a few times with SSH and it made my life easier. So, when I saw that we can use SSH to login remotely to my server, I had to try it. I created my SSH key pair with Git Bash but the first attempts did not work well for me. The server did not recognize my keys. Another problem I encountered was the weird behavior of the web console. The text I pasted on the console seems to not be the text I copied: Some characters got capitalized and others just got transformed to different characters. I am pretty sure that there is a good explanation behind that behavior but I did not try to understand. By the way, I don't like consoles that I cannot custumize the appearence. So, I was trying at all cost to go around the ugly web console to set up my server. A tool I used regulary to connect to my school server is Putty. It is the tool that saved my journey. 

##### My Relief
After I went over a few online articles about Ubuntu, SSH, Putty, PuttyGen, I followed these steps to create my droplet: 
  - Download [Putty and PuttyGen](https://www.putty.org/)
  - Generate a SSH public/private keys pair with PuttyGen
  - Add the public key the droplet (at creation)
  - Change the server's name
  - Test the connection with Putty

##### Alternative (using Git Bash)

As I said previously, there many ways of solving problems in software world. Putty save my life in this project but I still felt that I was on control of the process. I restarted over the same exercise using the Git Bash to generate and connect to my server. So, I:

  - created the SSH key with 
  ```bash
  ssh-keygen -b 4096 -t rsa -C "my@email.com"
  ```
  in the hidden ssh folder on my local machine
  - copied the public key to the new droplet
  - add the new ssh identity to the authorized identities list with 
  ```bash
  eval $(ssh-agent)
  ```
  because I already had multiple id file in my .ssh folder
  - connected with 
  ```bash
  ssh root@hostname
  ```

  I beleive that this solution is better than the previous one because you don't need extra tools Putty and PuttyGen for the preliminary setup of your server. I have to admit it, the Bash makes me look geeky and cool. 

##### What if You Want to Add a New SSH After Creating the Droplet?

I imagined a scenario in which I want to add a new SSH key to a server that is been created. So, I created a droplet without adding a SSK key at the creation just to test this scenario. then I:
  - generated a new pair of keys using ssh-keygen on my local machine
  - transfered the public key using 
  ```bash
  ssh-copy-id -i ~/.ssh/newkey.pub root@hostname
  ```
  In this step, I was prompted for the password of my server that Digital Ocean sent to me via email. Once the password is verified and the default password changed, the new key was added to the OpenSSL authorized_keys file.
  - tested the new key with ssh root@hostname. It did not work at first because I had multiple keys in my local ssh folder. So, I had to add the new SSH identity to the ssh identities list. The command 
  ```bash
  ssh-add ~/.ssh/privatenewkey
  ``` 
  should do the job, but it did not work at my first run because the ssh agent was not running. I started the ssh agent with 
  ```bash
  eval $(ssh-agent)
  ```
  then try to add the identity again. It worked just fine! I restarted the Bash console and try to login to my server and it went ok without asking for a password. This part of the server setup was completed.

### Create a Non Root User for Your Droplet

It is recommended to create a non root user for server in order to control what the user can or cannot do. Recall that the root user is a non human superuser. it has little restrictions than a user-created user. If you have a non root user, you can grant it the ability ot perform actions as superuser. You will be prompted for the password if you want to perform such actions. This is to make sure that you are an actual human and you are really who you say you are.

##### Let's do it

  - Add a new user called shoudou
  ```bash
  adduser shoudou
  ```
I was prompted for the password of the new user account and other personal information. Some of them were optional.
  - Set new user privileges. Let's give our new user the ability to perform some tasks as root.
  ```bash
  usermod -aG sudo shoudou
  ```
  - Login with as shoudou. We can just switch to the new user account instead of logging out and relogging in.
  ```bash
  sudo su - shoudou
  ```
  - Since it is a brand new user, we need to set up the a new SSH key for the account using the steps seen previously. It should be simple this time because you already have access to the server via the root user. You can just generate the SSH key locally and copy it and paste in 
  ```bash
   ~/.ssh/authorized_keys 
  ``` 
  If that file does not exist yet, you should create it and set its permission. If the ~/.ssh folder does not have the permission 700 and authorized_keys files does not have the permission 600, it is not possible to login using ssh.
  -  Changing the file's permissions
  ```bash
  chmod 600 ~/.ssh/authorized_keys
  ```
  - Test SSH service
  ```bash
  ssh -i ~/.ssh/id_rsa_shoudou shoudou@hostname
  ```

### Setting up Github in our Droplet

At this stage, I quickly understood that cloning my repositories directly from the droplet would be the easiest way to get my application files on the server. I was surprised that I could not just clone my repository like the way I do on my windows computer. I found out that I had to link my server with Github by setting a SSH. My concern was how I was going to transport the key easily from the server to my local machine. I am not a big fan of copying text from the terminal as I usually use ctrl+C and end up killing my running processes. I used WinSCP and worked fine and it was the easiest way to download my file. I used also sftp to download my key from the server to my local machine and that  method seems to be the method I am going to be using because it should work in most operating system, eaiser, and does not require additional tools. Long story short, here is how I did to set up the Github SSH.

  - Generate a pair of SSH keys on my server
  - Download the public key to my local machine
  ```bash
  sftp shoudou@hostname
  ```
  ```bash
  get .ssh/id_rsa_github.pub
  ```
  - Add key to my Github account. This is done in my account's Settings.

Now I am set to do great things with my half way set server. PowPow!!!


