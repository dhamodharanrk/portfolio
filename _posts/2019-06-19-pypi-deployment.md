---
title: PyPI Deployment
tags: [Python,Deployment]
style: border
color: secondary
description: "My First Package on PyPI"
---

# My First Package on PyPI

Okay, Lets get started,

## What is PyPI?

The Python Package Index (PyPI) is a repository of software for the Python programming language it helps you to find and install software developed and shared by the Python community. Package authors use PyPI to distribute their software.

## Initial Steps involved

To update the package on index, there are several steps needs to be followed in the development phase.

#### 1. Code should be depolopyment ready
Keeping in the mind that code will work globally

#### 2. Should follow the python package struture

There are some standared files required in order to create proper package struture . A sample structure shown below

```rst
/packaging_tutorial
  /example_pkg
    __init__.py
  setup.py
  LICENSE
  README.md
```
#### 3. Creating required files

There are some files required by PyPi and for packing the code. for example  __init__.py file , readme.md and LICENCE files along with setup.py

#### 4. Creating setup.py

setup.py is the build script for setuptools. It tells setuptools about your package (such as the name and version) as well as which code files to include

A sample code look like,

    import setuptools
    
    with open("README.md", "r") as fh:
        long_description = fh.read()
    
    setuptools.setup(
        name="example-pkg-your-username",
        version="0.0.1",
        author="Example Author",
        author_email="author@example.com",
        description="A small example package",
        long_description=long_description,
        long_description_content_type="text/markdown",
        url="https://github.com/pypa/sampleproject",
        packages=setuptools.find_packages(),
        classifiers=[
            "Programming Language :: Python :: 3",
            "License :: OSI Approved :: MIT License",
            "Operating System :: OS Independent",
        ],
    )

## Deploment on PyPI

Below steps are used to deploy the deployment ready project on PyPI index.

#### Creating accound on PyPi 

PyPi comes with two index services as below. One used to test the packages without affecting the live index. Another is a live index which is a global index

- test.pypi (testing envrionment)
- pypi (Live index)

#### Required Libraries

There are two libraries which is going to help us to deploy the package on cloud.

`python -m pip install --user --upgrade **setuptools wheel**`

` python -m pip install --user --upgrade **twine**`

#### Generating the Package file

Using below command on root directory will generate the package file for uploading based on setup.py file which we configured earlier

`python setup.py sdist bdist_wheel`

Once its done, there are some extra folder created on the root directory. 

```rst
dist/
  example_pkg_your_username-0.0.1-py3-none-any.whl
  example_pkg_your_username-0.0.1.tar.gz
```
You can locally test the package by installing using,

`pip install example_pkg_your_username-0.0.1.tar.gz`

#### Uploading on global index 

Using below command on root directory you can upload it on** test index** (ie.https://test.pypi.org/)

`python -m twine upload --repository-url https://test.pypi.org/legacy/ dist/*`

Follow the instructions on the screent to continue (You will be asked for User Name and password)

**Note :** Live and Test are two different index that means you need two sepearte logins.

To upload on **Live index** (ie.https://pypi.org/),

`python -m twine upload dist/*`

#### Installing using PIP

If you need to install it from the test index then,

`pip install -i https://test.pypi.org/simple/<PackageName>`

To Install on live index then use,

`pip install <PackageName>`

Great work! You have succesully indexed your first package or your own library on PyPI.


### Commands

python -m pip install --user --upgrade setuptools wheel

python setup.py sdist bdist_wheel

python -m pip install --user --upgrade twine

python -m twine upload --repository-url https://test.pypi.org/legacy/ dist/*

python -m twine upload dist/*


