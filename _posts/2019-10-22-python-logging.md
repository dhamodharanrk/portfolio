---
title: Simple Log functionality to Python Codes
tags: [Python]
style: fill
color: secondary
description: "Creating Log File Python"
---
# Creating Log File Python


Logging is an incredibly important feature of any application as it gives both programmers and people supporting the application key insight into what their systems are doing. Without proper logging we have no real idea as to why our applications fail and no real recourse for fixing these applications

    import logging
    from logging.handlers import RotatingFileHandler
    
    logFile = 'log.log'
    USER = 'root'
    formatter  = logging.Formatter('%(asctime)s %(levelname)s %(funcName)s(%(lineno)d) %(message)s')
    handler = RotatingFileHandler(logFile, mode='a', maxBytes=5*1024*1024, backupCount=2, encoding=None, delay=0)
    handler.setFormatter(formatter)
    handler.setLevel(logging.INFO)
    log = logging.getLogger(USER)
    log.setLevel(logging.INFO)
    log.addHandler(my_handler)
    
    log.info("This is test statement")
	
	

[Read More][1]
[1]: https://tutorialedge.net/python/python-logging-best-practices/ "Read more"
