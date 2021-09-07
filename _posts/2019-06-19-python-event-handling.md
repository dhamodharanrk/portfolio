---
title: Python Event Handling
tags: [Python,Deployment]
style: border
color: secondary
description: "A simple implementation of python event handling"
---

An example implementaion of Python event management

```python
class Observer():
    _observers = []
    def __init__(self):
        self._observers.append(self)
        self._observed_events = []
    def observe(self, event_name, callback_fn):
        self._observed_events.append({'event_name' : event_name, 'callback_fn' : callback_fn})

class Event():
    def __init__(self, event_name, *callback_args):
        for observer in Observer._observers:
            for observable in observer._observed_events:
                if observable['event_name'] == event_name:
                    observable['callback_fn'](*callback_args)

class Room(Observer):
    def __init__(self):
        print("Room is ready.")
        Observer.__init__(self)
    def someone_arrived(self, who):
        print(who + " has arrived!")
    def someone_left(self, who):
        print(who + " has left!")

# Monitoring some function
room = Room()
room.observe('someone arrived',  room.someone_arrived)

# Fire some events
# Event('someone left',    'John')
Event('someone arrived', 'Lenard') # will output "Lenard has arrived!"
# Event('someone Farted',  'Lenard')
```
