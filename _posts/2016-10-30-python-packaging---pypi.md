---
layout: post
title: "Python packaging - PyPi"
description: ""
category: old_blog
tags: []
---

How to test if packaging of the application works. The easy solution would be to
use [testpypi](https://testpypi.python.org/pypi) site. I myself had used it only
very few times. But this post I'll keep it as a documentation/reference for
myself for any future activities.


### Short version

In short what to do(to use testpypi):

* #### Create setup.py

* #### Create account in [PyPi](https://pypi.python.org/) website

* #### Create ~/.pypirc file

* #### Register the package:

```bash
python setup.py register -r testpypitest
```

* #### Build binary package and upload the package

```bash
python setup.py bdist upload -r testpypitest
```

### Long version:

#### How to create a setup.py script:

Create a sample project:

```
<root>
      |---hello.py
      |---setup.py
```


```python
from setuptools import setup

setup(
    name = 'hello',
    version = '0.1',
    url = 'http://testpackage.com'
    author = 'Author Name',
    author_email = 'iam@author.com',
    py_modules = ['hello'],
    entry_points = '''
        [console_scripts]
        hello=hello:hello
    '''
)
```

Arguements given to the setup function are basically some metadata, of the
project.

name                : Name of the project \\
version             : Version of the project \\
url                 : Url for the project \\
packages            : List of created packages to be included in the package build \\
py_modules          : All the individual python files to be included \\
install_requires    : External packages required \\
entry_points        : This gives a maping of the exe name to the funtion inside out module

Details regarding other options can be found [here](setuptools.readthedocs.io/en/latest/setuptools.html).


If we had a complete package with multiple modules in it:

```
<root>
      |--- testPackage
      |    |
      |    |--- __init__.py
      |    |--- module1.py
      |    |--- module2.py
      |   
      |--- setup.py
```


```python
from setuptools import setup

setup(
    name = 'test_package',
    version = '0.1',
    url = 'http://testpackage.com'
    author = 'Author Name',
    author_email = 'iam@author.com',
    packages = ['testPackage'],
    include_package_data = True,
    install_requires = [],
    entry_points = '''
        [console_scripts]
        hello=module1:hello
    ''',
)
```


#### Creating `~/.pypirc` file:

```
[distutils]
index-servers=
    pypi
    pypitest

[pypitest]
repository = https://testpypi.python.org/pypi
username = <username>
password = <password>
```


#### Registering, building and uploading the package binary


Once we have all pre-requisite setup done, now we'll go forward to registering
and uploading the package to TestPyPi site.

```python
python setup.py register -r testpypitest
python setup.py bdist upload -r testpypitest
```

if the building a binary distribution(option: bdist) and uploading the binary
package(option: upload) are not used in a chaining way it could result the error
as given below. So we will chain the commands and create the binary distribution
and upload it:

```bash
[abijith@abijithkp home]$ python setup.py upload -r pypitest
running upload
error: No dist file created in earlier command
```


#### Updating the code and making new releases

We have to increase the version number before updating

#### Deleting the package from PyPi index

To delete the project, login to your testpypi account. After selecting your account, remove the package. The site wil lask for confirmation.
