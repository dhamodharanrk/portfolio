---
title: Some Basic Python codes
tags: [Python]
style: border
color: info
description: "Basic Python codes"
---


# For Solving Encoding Issues

```python
-*Style-1*-
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
-*Style-2*-
chunk = chunk.decode('unicode-escape')
chunk = chunk.encode('ascii', 'ignore')
-*Style-3*-
# -*- coding: utf-8 -*-
```
# Batch Install or Uninstall Using PIP (Python)

```python
create a list of libs (pip freeze > file.txt)
To Uninstall (pip uninstall -y -r file.txt)
To Install (pip install -r file.txt)
```
# Calculating Program execution time
```python
import time
start = int (time.time() * 1000)
<code>
end = int (time.time() * 1000)
print (end - start) / 1000
```

# To get Current User Name Using pwd/getpass

```python
try:
    import pwd
except ImportError:
    import getpass
    pwd = None
def current_user():
    if pwd:
        return pwd.getpwuid(os.geteuid()).pw_name
    else:
        return getpass.getuser()
user_name = current_user()
```

# For Else Statement
```python
data_list =['a','b','c']
for val in data_list:
    if val=="aA":
        print "Match Found!"
        break
else:
    print "No Match Found!"
```
