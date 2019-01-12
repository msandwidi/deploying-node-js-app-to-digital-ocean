# Deploying a NodeJS App to Digital Ocean

In this article, I am trying to write down my frustrations so I hope to not feel them again when I will try to get a NodeJS app to run on Digital Ocean Droplets with all the necessary tools to make it a production grade app.

In th past, I have always used Heroku for hosting my NodeJS applications. I truly loved it because it is so easy work with. With my Github Student Pack accont, I got to create more applications on Heroku than before. I never struggle to deploy or manage any of my apps. At least, until I tried using NodeJS child processes to run multiple instances of my app. I was very excited when I saw my test app running 8 instances. No problem was reported until tried a much bigger application. It is reported on the console that the memory available was not enougth for my application. In my readings I learned that I should not try to run multiple processes base on the calculation of physical number of CPUs. I figured out what was going on and I and fixed the issue. I learned how Heroku calcultes the actual number of CPUs based on the memory of your dyno. I started wondering what else was keept from me. So, I started exploring other hosting options and comparing services. Digital Ocean came accross multiple times then I decided to look into it. I used my Github Student Pack account to got a $50 coupon when I signed up. Everything went smoothly at the begining. I learned few Ubuntu, SSH, and SFTP commands then I started exploring how I can benefit from Digital Ocean. I mostly work with NodeJS. So I decided to deploy a production ready NodeJS appication on Digital Ocean. The struggle started at that moment. 

My first few attempts were faillures and frustrations because I was just copying commands arround the different channels on Digital Ocean and pasting on the browser console. I did not really (try to) understand the commands. I just wanted to see a working hello world NodeJS application. Five hours after I started my hello world project, I got my NodeJS application to run. I was relief to see the hello world on the console. I let everything settle down for about 3 hours before I restart the project again but with the objective of understanding the overall process. 

The following are the steps I took to get my application to run on a Digital Ocean droplet. 

## Disclamer

There are many of deploying a NodeJS application to Digital Ocean. This is in no mean the best way of doing it. As a I said in the section above, I am just writing down the frustations I felt when I first tried to deplay my NodeJS appliation to Digital Ocean. The article is not intended also to make a recommendation of any service provider. You can choose whatever server fit your needs and budget.

## How I Wanted My NodeJS App to Be

NodeJS application development is so flexible that no one can claim to have the best application structure. At least, I beleive so. But many developers seem to agree with some features that a good NodeJS Production application should have. We are not going to argue about that today. 

These are the important features/behaviors I wanted my app to have.
  - Persistence using MongoDB
  - Send emails with Sengrid
  - Schedule talks with Agenda
  - The app should restart automatically if the Droplet restart
  - The app should run on multiple child process
  - The app should run behind Nginx server
  - A process should try restarting when it dies unexpectidely