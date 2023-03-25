# Setup DIUN (Docker Image Update Notifier) On A Synology NAS To Receive Notifications When A Docker Image Is Updated

## Description

This repository will provide you details on how to run DIUN (Docker Image Update Notifier) on your Synology NAS so you can receive updates when Docker images are updated for your running containers.  You'll be using docker-compose to spin up the DIUN container and I've included a few specifics that I use in my setup including the following:

1. Health check notifications, using Healthchecks.io, to get notified if DIUN is down.
2. Pushover for push notifications sent to my iPhone.
3. Email notifications using Gmail and an app password (if needed).

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
7. Setup Healthchecks.io to monitor the status of DIUN container. OPTIONAL
   1. Go to https://healthchecks.io and Sign In or Sign Up for an account.
   2. Once logged in select Add Check or edit an existing check that you would like to use and give it an appropriate name.
   3. You'll need to copy UUID, which is the hexadecimal number that you see at the end of the Ping URL.
   4. REFERENCE - I cover Healthchecks.io to monitor your Synology NAS in this YouTube video -> https://youtu.be/_7oRJtHUUpw
8. Setup Pushover to send push notifications to your mobile device (iPhone in my case). OPTIONAL
   1. Go to https://pushover.net and Login or Signup for an account.
   2. Once logged in go to the Your Applications section and click Create an Application/API Token.
   3. This brings up the Create New Application/API Token window where you'll need to enter in a Name, check the box that you agree to all the terms and click Create Application.
   4. You'll need to copy the API Token/Key for Application that was just created.
   5. Also, you'll need to copy Your User Key from the home page (click on Pushover from the top navigation bar to go to the home page).
9. For email notifications through Gmail you'll need an App Password if you've enabled 2-Step verification for your Google Account. OPTIONAL
   1. The steps to create an App Password are documented in this write up by Google -> https://support.google.com/accounts/answer/185833?hl=en and I also go through the steps in the Youtube video that is associated with this GitHub project listed above.
10. Once steps 7 - 8 are completed, assuming you setup the items in each step, you can edit the .env file and replace the filler text with your Healthchecks.io, Pushover and Gmail information. Also change the TZ entry to your timezone and the DIUN_WATCH_SCHEDULE to your specifications using the cron scheduling format (default is set for 8:00am every day).
```
TZ=Pacific/Honolulu
DIUN_WATCH_SCHEDULE='0 8 * * *'
DIUN_WATCH_HEALTHCHECKS_UUID=YOUR_HEALTHCHECK_ID_UUID
DIUN_NOTIF_PUSHOVER_TOKEN=YOUR_PUSHOVER_APP_API_TOKEN
DIUN_NOTIF_PUSHOVER_RECIPIENT=YOUR_PUSHOVER_USER_KEY
DIUN_NOTIF_MAIL_USERNAME=YOUR_GMAIL_EMAIL_ADDRESS
DIUN_NOTIF_MAIL_PASSWORD=YOUR_GMAIL_APP_PASSWORD
DIUN_NOTIF_MAIL_FROM=PROBABLY_YOUR_GMAIL_EMAIL_ADDRESS
DIUN_NOTIF_MAIL_TO=EMAIL_ADDRESS_TO_RECEIVE_NOTIFICATION
```
11. You should now be able to create and start the DIUN container.
```
sudo docker-compose up -d
```
12.  Once DIUN is running you can confirm that notifications are working by running the following commands.

Command to connect to the DIUN container.
```
sudo docker container exec -it diun /bin/sh
```
Command to send a test notification from within the container.
```
diun notif test
```
If you receive the test notification messages you should be all set.
