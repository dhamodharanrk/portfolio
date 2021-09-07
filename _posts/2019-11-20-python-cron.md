---
title: Managing Cron Jobs Using Python
tags: [Linux,Python]
style: border
color: secondary
description: "Managing Cron Jobs Using Python"
---

**What Is Cron?**

During system administration, it's necessary to run background jobs on a server to execute routine tasks. Cron is a system process which is used to execute background tasks on a routine basis. Cron requires a file called crontab which contains the list of tasks to be executed at a particular time. All these jobs are executed in the background at the specified time.

To view cron jobs running on your system, navigate to your terminal and type in:

`crontab -l`

The above command displays the list of jobs in the crontab file. To add a new cron job to the crontab, type in:

`crontab -e`

The above command will display the crontab file where you can schedule a job. Let's say you have a file called hello.py which looks like:

`print "Hello World"`

Now, to schedule a cron job to execute the above script to output to another file, you need to add the following line of code:

`50 19 * * * python hello.py >> a.txt`

The above line of code schedules the execution of the file with output to a file called a.txt. The numbers before the command to execute define the time of execution of the job. The timing syntax has five parts:

    minute
    hour
    day of month
    month
    day of week
    Asterisks(*) in the timing syntax indicate it will run every time. 

**Introducing Python-Crontab **

python-crontab is a Python module which provides access to cron jobs and enables us to manipulate the crontab file from the Python program. It automates the process of modifying the crontab file manually. To get started with python-crontab, you need to install the module using pip:

`pip install python-crontab`

Once you have python-crontab installed, import it into the python program.

`from crontab import CronTab`

**Writing Your First Cron Job**

Let's use the python-crontab module to write our first cron job. Create a Python program called writeDate.py. Inside writeDate.py, add the code to print the current date and time to a file. Here is how writeDate.py would look:

    import datetime 
    with open('dateInfo.txt','a') as outFile:
        outFile.write('\n' + str(datetime.datetime.now()))
	
Save the above changes.

Let's create another Python program which will schedule the writeDate.py Python program to run at every minute. Create a file called scheduleCron.py.

Import the CronTab module into the scheduleCron.py program.

`from crontab import CronTab`
Using the CronTab module, let's access the system crontab.

`my_cron = CronTab(user='your username')`

The above command creates an access to the system crontab of the user. Let's iterate through the cron jobs and you should be able to see any manually created cron jobs for the particular username.

    for job in my_cron:
        print job
		
Save the changes and try executing the scheduleCron.py and you should have the list of cron jobs, if any, for the particular user. You should be able to see something similar on execution of the above program:

`50 19 * * * python hello.py >> a.txt # at 5 a.m every week with:`

Let's move on with creating a new cron job using the CronTab module. You can create a new cron by using the new method and specifying the command to be executed.

`job = my_cron.new(command='python /home/jay/writeDate.py')`

As you can see in the above line of code, I have specified the command to be executed when the cron job is executed. Once you have the new cron job, you need to schedule the cron job. 

Let's schedule the cron job to run every minute. So, in an interval of one minute, the current date and time would be appended to the dateInfo.txt file. To schedule the job for every minute, add the following line of code:

`job.minute.every(1)`

Once you have scheduled the job, you need to write the job to the cron tab.

`my_cron.write()`

Here is the scheduleCron.py file:

```python
from crontab import CronTab 
my_cron = CronTab(user='roy')
job = my_cron.new(command='python /home/roy/writeDate.py')
job.minute.every(1)
my_cron.write()
```
Save the above changes and execute the Python program.

`python scheduleCron.py`

Once it gets executed, check the crontab file using the following command:

`crontab -l`

The above command should display the newly added cron job.

`* * * * * python /home/roy/writeDate.py`

Wait for a minute and check your home directory and you should be able to see the dateInfo.txt file with the current date and time. This file will get updated each minute, and the current date and time will get appended to the existing content.

**Updating an Existing Cron Job**

To update an existing cron job, you need to find the cron job using the command or by using an Id. You can have an Id set to a cron job in the form of a comment when creating a cron job using python-crontab. Here is how you can create a cron job with a comment:

    job = my_cron.new(command='python /home/roy/writeDate.py', comment='dateinfo')
	

As seen in the above line of code, a new cron job has been created using the comment as dateinfo. The above comment can be used to find the cron job.

What you need to do is iterate through all the jobs in crontab and check for the job with the comment dateinfo. Here is the code:

    my_cron = CronTab(user='roy')
    for job in my_cron:
        print job
		
Check for each job's comment using the job.comment attribute.

    my_cron = CronTab(user='jay')
    for job in my_cron:
        if job.comment == 'dateinfo':
            print job
			

Once you have the job, reschedule the cron job and write to the cron. Here is the complete code:

    from crontab import CronTab 
    my_cron = CronTab(user='roy')
    for job in my_cron:
        if job.comment == 'dateinfo':
        job.hour.every(10)
        my_cron.write()
        print 'Cron job modified successfully'
		

Save the above changes and execute the scheduleCron.py file. List the items in the crontab file using the following command:

`crontab -l`

You should be able to see the cron job with updated schedule time.

`* */10 * * * python /home/jay/writeDate.py # dateinfo`

**Clearing Jobs From Crontab**

python-crontab provides methods to clear or remove jobs from crontab. You can remove a cron job from the crontab based on the schedule, comment, or command.

Let's say you want to clear the job with comment dateinfo from the crontab. The code would be:

    from crontab import CronTab 
    my_cron = CronTab(user='roy')
    for job in my_cron
        if job.comment == 'dateinfo':
            my_cron.remove(job)
            my_cron.write()
			

Similarly, to remove a job based on a comment, you can directly call the remove method on the my_cron without any iteration. Here is the code:

`my_cron.remove(comment='dateinfo')`

To remove all the jobs from the crontab, you can call the remove_all method.

`my_cron.remove_all()`

Once done with the changes, write it back to the cron using the following command:

`my_cron.write()`

**Calculating Job Frequency**

To check how many times your job gets executed using python-crontab, you can use the frequency method. Once you have the job, you can call the method called frequency, which will return the number of times the job gets executed in a year.

    from crontab import CronTab 
    my_cron = CronTab(user='roy')
    for job in my_cron:
        print job.frequency()
		

To check the number of times the job gets executed in an hour, you can use the method frequency_per_hour.

    my_cron = CronTab(user='roy')
    for job in my_cron:
        print job.frequency_per_hour()

To check the job frequency in a day, you can use the method frequency_per_day. 

**Checking the Job Schedule**

python-crontab provides the functionality to check the schedule of a particular job. For this to work, you'll need the croniter module to be installed on your system. Install croniter using pip:

`pip install croniter`

Once you have croniter installed, call the schedule method on the job to get the job schedule.

`sch = job.schedule(date_from=datetime.datetime.now())`

Now you can get the next job schedule by using the get_next method.

`print sch.get_next()`

Here is the complete code:

    import datetime
    from crontab import CronTab 
    my_crons = CronTab(user='jay')
    for job in my_crons:
        sch = job.schedule(date_from=datetime.datetime.now())
        print sch.get_next()
		

You can even get the previous schedule by using the get_prev method.
