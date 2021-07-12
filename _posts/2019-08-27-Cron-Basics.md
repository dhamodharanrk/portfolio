---
title: Getting Started with Cron Jobs
tags: [Python,Linux]
style: border
color: info
description: "Getting Started with Cron Jobs"
---


# Cron Job Basics

Cron is a system process which is used to execute background tasks on a routine basis. Cron requires a file called crontab which contains the list of tasks to be executed at a particular time. All these jobs are executed in the background at the specified time

To view cron jobs running on your system, navigate to your terminal and type in:

`crontab -l`

The above command displays the list of jobs in the crontab file. To add a new cron job to the crontab, type in:

`crontab -e`

The above command will display the crontab file where you can schedule a job. Let's say you have a file called hello.py which looks like:

`print "Hello World"`

Now, to schedule a cron job to execute the above script to output to another file, you need to add the following line of code:

`50 19 * * * python hello.py >> a.txt`

The above line of code schedules the execution of the file with output to a file called a.txt. The numbers before the command to execute define the time of execution of the job. The timing syntax has five parts:

1. minute
2. hour
3. day of month
4. month
5. day of week

Asterisks(*) in the timing syntax indicate it will run every time. 

    * * * * * command to be executed
    - - - - -
    | | | | |
    | | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
    | | | ------- Month (1 - 12)
    | | --------- Day of month (1 - 31)
    | ----------- Hour (0 - 23)
    ------------- Minute (0 - 59)
	

