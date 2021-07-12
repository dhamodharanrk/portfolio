---
title: Python time operation to find shop open/close
tags: [Python,Logics]
style: border
color: info
description: "Time operation to find the shop is open or closed"
---

Is my shop is open ?

Simple implementation to find whether the shop is open based on time given.

For example , 10:00AM to 06:00PM  this compared with Current time and returns  "Opened" or "Closed"

### Steps
-  Need to Understand the time


```python
import calendar
import datetime
working_hours = {'friday':'06:00PM-10:00PM'}

def convert_to_24(time_string):
    if time_string[-2:] == "AM" and time_string[:2] == "12":
        return "00" + time_string[2:-2]
    elif time_string[-2:] == "AM":
        return time_string[:-2]
    elif time_string[-2:] == "PM" and time_string[:2] == "12":
        return time_string[:-2]
    else:
        return str(int(time_string[:2]) + 12) + time_string[2:5]

def isOpen(startTime, endTime, x):
    if startTime <= endTime:
        return startTime <= x <= endTime
    else:
        return startTime <= x or x <= endTime

current_time = datetime.datetime.now().time()
current_day = calendar.day_name[datetime.datetime.now().weekday()]

current_open_hours = working_hours.get(str(current_day).lower())
data_split = str(current_open_hours).split('-')

startTime = datetime.time(int(str(convert_to_24(data_split[0])).split(":")[0]), int(str(convert_to_24(data_split[0])).split(":")[0]), 0)
endTime = datetime.time(int(str(convert_to_24(data_split[1])).split(":")[0]), int(str(convert_to_24(data_split[1])).split(":")[0]), 0)
print(isOpen(startTime,endTime,current_time))
```
