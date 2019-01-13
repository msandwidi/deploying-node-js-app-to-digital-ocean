# Deploying a NodeJS App to Digital Ocean

In this article, I am trying to write down my frustrations hoping to not feel them again when I will try to get a NodeJS app up and running on Digital Ocean Droplets with all the necessary tools to make it a production grade app. Let me specify that the term "production grade" is relative to what I was trying to do with my application. 

In the past, I have always used Heroku for hosting my NodeJS applications. I truly like Heroku because it is so easy to work with. With my Github Student Pack account, I got to create more applications on Heroku than before. I never struggle to deploy or manage any of my apps. At least, until I tried using NodeJS child processes to run multiple instances of my application. I was very excited when I saw my test app running on more than one thread. No problem was reported until tried a much bigger application. It is reported on the console that the memory available was not enougth for my application. In my readings I learned that I should not try to run multiple processes base on the calculation of physical number of CPUs. I figured out what was going on and I fixed the issue. I learned how Heroku calculates the actual number of CPUs based on the memory of your dyno. I started wondering what else was keept from me. So, I started exploring other hosting options and comparing services. I came accross Digital Ocean multiple times. So, I decided to look into it. I used my Github Student Pack account again to get a $50 coupon when I signed up. Everything went smoothly at the begining. I learned few Ubuntu, SSH, and SFTP commands then I started exploring how I can benefit from Digital Ocean. I mostly work with NodeJS. So I decided to deploy a production ready NodeJS appication on Digital Ocean. The struggle started at that very moment. 

My first few attempts were faillures and frustrations because I was just copying commands arround the different channels on Digital Ocean and pasting on the browser console. I did not really (try to) understand the commands. I just wanted to see a working hello world NodeJS application. Five hours after I started my hello world project, I got my NodeJS application to run locally on the server. I was relieved to see the hello world on the console. I let everything settle down for about 3 hours before I restarted the project again but with the objective of understanding the overall process. 

The following are the steps I took to get my application to run on a Digital Ocean droplet. 

## Disclamer

There are many  ways of deploying a NodeJS application to Digital Ocean. This is in no mean the best way of doing it. As a I said in the section above, I am just writing down the frustations I felt when I first tried to deploy my NodeJS appliation to Digital Ocean. The article is not intended also to make a recommendation of any service provider. You can choose whatever service that fits your needs and budget.

## How I Wanted My NodeJS App to Be

NodeJS application development is so flexible that no one can claim to have the best application structure. At least, I beleive so. But many developers seem to agree with some features and pratices that a good NodeJS Production application should have. We are not going to argue about that today. 

These are the important features/behaviors I wanted my NodeJS app to have.
  - Persistence using MongoDB
  - Send emails
  - Schedule future tasks
  - Deleguate some tasks to background workers
  - App restarts automatically if the Droplet restarts
  - App runs on multiple child processes depending on the number of CPUs available 
  - App runs behind a reverse proxy server
  - child process tries to restart when it dies unexpectidely
  - more ...

I am going to stick only on putting my application behind a reverse proxy server in this article. if you want to see a sample program that implements some of the behaviors listed above, visit my [Github](https://github.com/shoudou) profile to see what you can find.

## Let's Get Started

### Creating a Brand new Droplet

##### My Struggle 

Creating a droplet seems to be easy. It is actually easy because the interface is friendly and the whole process takes only a few steps. So, why did I struggle to create simple droplet? Actually, I created my first droplets successfully. But, I wanted to understand what was going on behind the scene, what flexibility margin the plateform gives me, and how I can dictate my own rules for most of the featuress instead of letting Digital Ocean setting up everything for me. Let's be Engineers! 

I worked a few times with SSH and it made my life easier. So, when I saw that we can use SSH to login remotely to my droplet, I had to try it. I created my SSH key pair with Git Bash but the first attempts did not work out well. The server was not recognizing my keys. I encountered a weird behavior of the web console of my droplet. The text I paste on the console seems to not be the text I copied: Some characters got capitalized and others just transform different character. I am pretty sure that there is a good explanation behind that behavior but I did not try understand it because I have always hated console that you cannot custumize. I was trying at all cost to go around the web console to set up my server. A tool I used regulary to connect to my school server is Putty. It is the tool that saved my journey. 

##### My Relief
After I went over few onlines articles about SSH, Putty, and PuttyGen, I followed these steps to create my droplet: 
  - Download Putty and PuttyGen
  - Generate a SSH public/private keys pair
  - Add the public key the droplet
  - Change the server's name
  - Test remote connection with Putty
