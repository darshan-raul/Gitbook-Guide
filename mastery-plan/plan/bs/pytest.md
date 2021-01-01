# Pytest

{% embed url="https://www.guru99.com/pytest-tutorial.html" %}

Using the above reference to implement pytest in my code. 

Iam currently working on my CLI application for AWS. But it is getting frustrating and  illogical to use 'print' to troubleshoot the code. Also as I plan to create a package out of this code ultimately , Hence I desperately needed to create some test scenarios and put them to 'test' :p

So here we go :

{% embed url="https://github.com/darshan-raul/Python-CLI-for-AWS" %}

This is the code and I am going to start slow , add couple of tests and then ultimately will try to create a  total test coverage\("pretty sure there is a term for this which I dont know yet\)

{% hint style="info" %}
First of all , I created a folder just for all the test files to reside in. Lets call it 'tests'
{% endhint %}

### Configuration:

There are some simple ground rules while creating a pytest file or function:

1.  By default pytest only identifies the file names starting with **test\_** or ending with **\_test** as the test files.
2.  Pytest requires the test method names to 🌟 _**start**_ 🌟 with **"test**." All other method names will be ignored even if we explicitly ask to run those methods.

```text
test_login.py - valid
login_test.py - valid
testlogin.py -invalid
logintest.py -invalid
```

```text
def test_file1_method1(): - valid
def testfile1_method1(): - valid
def file1_method1(): - invalid	
```

### Running test's

```text
py.test
```

This will run all the filenames starting with test\_ and the filenames ending with \_test in that folder and subfolders under that folder.

To run tests only from a specific file, we can use py.test &lt;filename&gt;

```text
py.test test_sample1.py
```

`pytest` allows you to use the standard python `assert` for verifying expectations and values in Python tests

```text
def f():
    return 3


def test_function():
    assert f() == 4
```



### Sample file:

```text
import boto3

s3=boto3.client('s3')

def test_getbuckets():
    bucketlist=[]
    buckets=s3.list_buckets()
    for i in buckets['Buckets']:
        bucket= i['Name']
        
        bucketlist.append(bucket)
    #print(bucketlist)
    assert bucketlist!=[] 
    
```

Nothing fancy just needs to check if the bucketlist is empty or not. If it isn't the code ran successfully.

**Output:**

![1 passed 2 warnings as the end result](../../../.gitbook/assets/image%20%2877%29%20%281%29.png)

Now I removed the s3 access for this AWS user and re-ran the test

![As expected it failed with 1 failed,2 warnings](../../../.gitbook/assets/image%20%28158%29.png)

![You will also see a F in front of the file](../../../.gitbook/assets/image%20%28125%29.png)

{% hint style="info" %}
Execute the test function with “quiet” reporting mode:
{% endhint %}

```text
$ pytest -q test_sysexit.py
.                                                                    [100%]
1 passed in 0.12 seconds
```



