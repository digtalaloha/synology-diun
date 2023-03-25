# Setup DIUN (Docker Image Update Notifier) On A Synology NAS To Receive Notifications When A Docker Image Is Updated

## Description

This repository will provide you details on how to run DIUN (Docker Image Update Notifier) on your Synology NAS so you can receive updates when Docker images are updated for your running containers.  You'll be using docker-compose to spin up the DIUN container and I've included a few specifics that I use in my setup including the following:

1. Health check notifications, using Healthchecks.io, to get notified if DIUN is down.
2. Pushover for push notifications sent to my iPhone.
3. Email notifications using Gmail and an app password.

### YouTube Video
I've also created a YouTube video that goes through the steps that I went through in configuring my Synology NAS with DIUN that you can find here -> VIDEO TO COME SHORTLY.

### Customize DIUN Further
If you would like to customize your DIUN setup beyond what I've covered in this repository you can download the docker-compose.yml file and visit the [DIUN]{https://crazymax.dev/diun/} website for details.  There are a bunch of other notification options and features that you can change to fit your environment. 

## Directions

### Prerequistes
On your Synology NAS you'll need the following setup:
1. Docker already running with containers that you would like to keep up-to-date.
2. Install the Git Server package.
3. Enable SSH.

### Setup Steps 
1. SSH into your Synology NAS.
```
ssh <admin account>@<IP address of Synology NAS>
```
2. Change directory into /volume1/docker. 
```
cd /volume1/docker
```
4. Clone this repository.
```
sudo git clone https://github.com/digtalaloha/synology-diun.git
```
5. Change directory into the newly created synology-diun directory.
```
cd synology-diun
```
6. Create host volume mount points that will be used by DIUN.
```
mkdir data
```
7. Setup Healthchecks.io to monitor the status of DIUN container.
   1. Go to https://healthchecks.io and Sign In or Sign Up for an account.
   2. Once logged in select Add Check or edit an existing check that you would like to use and give it an appropriate name.
   3. You'll need to copy UUID, which is the hexadecimal number that you see at the end of the Ping URL.

   * Reference - I cover Healthchecks.io to monitor your Synology NAS in this YouTube video -> https://youtu.be/_7oRJtHUUpw 
8. Setup Pushover to send push notifications to your mobile device (iPhone in my case).
   1. Go to https://pushover.net and Login or Signup for an account.
   2. Once logged in go to the Your Applications section and click Create an Application/API Token.
   3. This brings up the Create New Application/API Token window where you'll need to enter in a Name, check the box that you agree to all the terms and click Create Application.
   4. You'll need to copy the API Token/Key for Application that was just created.
   5. Also, you'll need to copy Your User Key from the home page (click on Pushover from the top navigation bar to go to the home page).
10. Gmail
