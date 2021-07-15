---
title: Sending email from Crontab
tags: [Linux]
style: fill
color: info
description: "Sending email from Crontab"
---

Cron is the Linux task scheduler that is responsible for making sure scripts run at their specified times. Cron is often used for things like, log rotation, backup scripts, updating file indexes, and running custom scripts. In the event a task runs into problems or errors Cron generally tries to email the local administrator of the machine. This means it tries to send an email to itself instead of an “internet accessible” email address like, ‘user@gmail.com’.

We can change this default behavior by changing the MAILTO variable.

[![](https://raw.githubusercontent.com/dhamodharanrk/examples/master/blog_files/mailto-cron.png)](https://raw.githubusercontent.com/dhamodharanrk/examples/master/blog_files/mailto-cron.png)

**Setting the MAILTO variable**

Cron relies on a simple text file to schedule commands. To edit this file just issue the crontab command:

`crontab -e`

To change the MAILTO variable just add `‘MAILTO=username@domain.com’ ` into the crontab file.

**Specify Email for Each Script**

If we don’t want all output to go to the same email address we can specify the output of a particular script to go to a different email address:

`59 */6 * * * script.sh | mail -s "Subject of Mail" someother@address.com`

**Email Alerts for All but One**

If you have a specific script in your crontab that you don’t want output or errors emailed to you, simply add, `‘>/dev/null 2>&1`’ to the end of the command.

`59 */6 * * * script.sh >/dev/null 2>&1`
