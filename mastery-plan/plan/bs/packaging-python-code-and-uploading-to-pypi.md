# Packaging python code and uploading to PyPi

Iam in the middle of creating a CLI application for AWS. Similar to awscli and other interesting cli application's around it :[https://github.com/donnemartin/awesome-aws\#cli](https://github.com/donnemartin/awesome-aws#cli), But in a more interactive way.

Here it is :

{% embed url="https://github.com/darshan-raul/Python-CLI-for-AWS" %}

It's still in its early phases hence I have not yet blogged about it. But before I could go any further in this project I wanted to know how to create a Pypi package so that some day someone would do  `pip install <my_cli_package_name>` 😉 

I referred these:

* [https://dzone.com/articles/executable-package-pip-install](https://dzone.com/articles/executable-package-pip-install)
* [https://python-packaging.readthedocs.io/en/latest/minimal.html](https://python-packaging.readthedocs.io/en/latest/minimal.html)
* [https://www.youtube.com/watch?v=z2YipGrbuKw](https://www.youtube.com/watch?v=z2YipGrbuKw)

**So here are the steps:**

###  First make sure that you have the following packages installed:

```text
sudo python -m pip install --upgrade pip setuptools wheel
sudo python -m pip install tqdm
sudo python -m pip install  --upgrade twine
```

###  Folder Structure:

![](../../../.gitbook/assets/image%20%2877%29.png)

1. Create a folder with the 'package-name' as the title
2. Create a file named `__init__.py`. This will establish the package. You can use other way's but this is a standard and preferred way.
3. Create a setup.py file which will be sued to create the pip wheel files .

{% code title="\_\_init\_\_.py" %}
```text
def main():
    print ('Hi there')
       

if __name__ == '__main__':
    main()
```
{% endcode %}

{% code title="setup.py" %}
```text
import setuptools

with open("README.md", "r") as fh:
    long_description = fh.read() ## add all the instructions here

setuptools.setup(
     name='<package-name',  
     version='<version_number_of_package',
     author="<your-name",
     author_email="<your-email>",
     description="<brief description for the program>",
     long_description=long_description,
   long_description_content_type="text/markdown",
     url="<github url of the package code",
      license='MIT',
      packages=['<package-name'],
      zip_safe=False
 )
```
{% endcode %}

That's all that is needed for a basic package and then you can expand on the same.

### Create a wheel file:

```text
python setup.py bdist_wheel
```

This will create the following folders:

![](../../../.gitbook/assets/image%20%28123%29.png)

### Test in local machine:

```text
pip install dist/<wheel name>
```

This will install your python package in your computer. Go ahead and use `'import package-name'`

### Create Pypi account:

{% embed url="https://pypi.org/account/register/" %}

You will receive a username and password. Save them as they will be needed to upload the package to Pypi.

### Create .pypric file

{% code title=".pypric" %}
```text
[distutils] 
index-servers=pypi
[pypi] 
repository = https://upload.pypi.org/legacy/ 
username =<username>
```
{% endcode %}

This file is essential for storing configuration of your Pypi account.

### Upload to PyPi:

```text
python -m twine upload dist/*
```

This will upload the package to PyPi:

![](../../../.gitbook/assets/image%20%2896%29.png)

Try `pip install pyawscli`

It works :\)



