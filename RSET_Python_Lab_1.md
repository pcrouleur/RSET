# Python Lab p1

## Prerequisites
### 1. Python
#### Mac
If you have a Mac and have the command line developer tools you should have python 3.9.7 installed already. If not you can just run python3 and it will give you an option to install the developer tools.
#### Windows
On Windows you can intall python from the Microsoft Store. That should put python in your path. 
#### Linux
On Linux you can install it from your repository (although if you are on <8 you will get a pretty old version but it will work).
#### Or Install from Python.org
```
https://www.python.org/downloads/
```

### 2. API Token
Create and save an API token from CDM01. Just to make it easy just get one with your users admin privileges. You can connect to another CDM if you want but you will get different results than what we do in the lab. 

### 3. Pycharm
If you want to work with a Python IDE, download and install the Pycharm community edition. This is optional. 
```
https://www.jetbrains.com/pycharm/download
```

## Create a virtual environment for the lab:
------------------------------------------------------------------------------------

### From a terminal window check python. 
Your python could be something different depending on the intall. It might be python3, python3.x, etc. If it is not in your PATH you may have to add that. 
```
python3.9 -V
```

### Then create the virtual envirnment:
cd to where you want the virtual environment
```
cd /home/rset_labs/python
```
Create the virtual environment using your python with whatever name you prefer. In this example it is 'pylabenv'
```
python3.9 -m venv pylabenv
```

Activate the environment (This can be added to your .bash_profile):
On Mac and Nix:
```
source /home/rset_labs/python/pylabenv/bin/activate
```
On Windows:
```
\home\rset_labs\python\pylabenv\scripts\activate.bat
```

Upgrade pip (optional):
```
pip install --upgrade pip
```

Install the Rubrik CDM module:
```
pip install rubrik_cdm
```
## Set your cdm address and token environment variable
On Mac/linux:
```
export rubrik_cdm_node_ip=10.8.48.201
export rubrik_cdm_token=<your api token>
```
On Windows: <br>
Set the environment variables with the System Control Panel

To make a persistent change to an environment variable on Windows using the System Control Panel:

1. Open the System Control Panel.
2. Select System.
3. Select Advanced System Settings.
4. Go to the Advanced tab.
5. Select Environment Variables....
6. Make your changes.

![Screen Shot 2022-07-08 at 3 08 45 PM](https://user-images.githubusercontent.com/26155271/178071336-5cba0315-00f1-4747-9ee9-ac9733c9ff0a.png)
![Screen Shot 2022-07-08 at 3 09 23 PM](https://user-images.githubusercontent.com/26155271/178071348-799e443b-c7f9-46d0-8664-d26c5ce9eab2.png)

# Lab Exercises Using the Python REPL
## Start the REPL

```
❯ python
Python 3.9.7 (v3.9.7:1016ef3790, Aug 30 2021, 16:39:15)
[Clang 6.0 (clang-600.0.57)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```
Import the Rubrik CDM Module
```
import rubrik_cdm
```
Create a Rubrik connection object
```
rubrik = rubrik_cdm.Connect()
```
Now we can do a "Get" using that connection
```
rubrik.get('v1','/cluster/me')
```

Certificate Validation
When connecting to a Rubrik cluster without certificate validation enabled you will receive the following warning message:
```
/Users/julianz/Python/py39/lib/python3.9/site-packages/urllib3/connectionpool.py:1013: InsecureRequestWarning: Unverified HTTPS request is being made to host '10.8.48.201'. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/1.26.x/advanced-usage.html#ssl-warnings
  warnings.warn(
```
This warning may be suppressed utilizing the urllib3 library and inserting the following code within your script:
```
import urllib3
urllib3.disable_warnings()
```
Where's the help?
```
 help(rubrik_cdm)

help(rubrik_cdm.api)
```
quit or q to exit help

Lets get the cluster info
```
info = rubrik.get('v1','/cluster/me')
```
There is a type function that will tell you what data type was returned
```
type(info)
<class 'dict'>
```
We can print that info dictionary in the REPL by just entering the variable name
```
info
```
You can then get individual values using the key
```
info['id']
```
Now lets get some details about a snappable. This time let's assign that to a variable
```
dbs = rubrik.get('v1','/oracle/db')
```
In the REPL we can see what is in the variable by entering the variable name. Out side of the REPL you would need to use a print() function
```
dbs
print(dbs)
```
Now that is an easy read, right? Let's fix that. There is a pretty print class we can use to format the output so it is more readable. 
```
import pprint
pp = pprint.PrettyPrinter(indent=2)
pp.pprint(dbs)
```
This is typical of the nested data types you will see from the REST endpoints. In order to access the data you need to know what data types you are accessing. Python provides the type function for that. 
```
type(dbs)
```
The first level is a dictionary. Let's get the keys and data type from that dict.
```
for key in dbs:
    print(key, type(dbs[key]))

```
Now let's see how many elements in the 'data' list. There is a len function for that. 
```
len(dbs['data'])
```

We can retrieve one element
```
dbs['data'][0]
```
Or pretty print the element
```
pp.pprint(dbs['data'][0])
```
How would we find the elements matching a criteria. We can loop through and look for matches. Lets get the data for database with the name 'etst' and 
```
for db in dbs['data']:
    if db['name'] == 'etst':
        pp.pprint(db)
```
There were 2 databases returned, we can add to the comparison to get just the databases on the host 'rp-ora-demo-01.perf.rubrik.com'
```
for db in dbs['data']:
    if db['name'] == 'etst' and db['standaloneHostName'] == 'rp-ora-demo-01.perf.rubrik.com':
        pp.pprint(db)
```
# Python IDE (Pycharm) -  Getting started

Start Pycharm and select "New Project"
Now create a project and set the project interpreter. 

![Screen Shot 2022-07-01 at 12 44 21 PM](https://user-images.githubusercontent.com/26155271/176961914-65aab968-8aec-415e-b022-4e5a085701ae.png)

Set a location and name for your project. 
Now you can create a new virtual envirnment for the project or you can use an existing environment.

You can select "Previously configured interpreter" then navigate to the python executable in your lab virtual environment. In my case it was "/users/julianz/Python/py39/bin/python3.9"
 On a windows box it might be "C:\Users\julianz\Python\py39\Scripts\python.exe"
 
Leave the "Create a main.py welcome script" checked and then hit "Create". You should get a screen that has something like this. You're theme might be different. This is GitHub Dark (Material). 
![Screen Shot 2022-07-01 at 2 23 55 PM](https://user-images.githubusercontent.com/26155271/176963651-43925e6b-cde5-4d1a-9dfe-3f95af3a5a12.png)
Now run the main.py script to make sure everything is working. Use the green arrow or the "Run" menu bar item. You should see the script output in the output window at the bottom. 
![Screen Shot 2022-07-01 at 2 26 30 PM](https://user-images.githubusercontent.com/26155271/176963973-07d0f403-738a-465a-993f-4a07369c5e1a.png)

Very often python scripts will be written with a main function that is called when the script is run. Then other scripts can import the script and use it's functions without having the main function run. It's not always necessary but it's here so you can see how that is done. 

Let's create a new script, set up the main function, and import those modules we used in the REPL. 

Go to "File" -> "New..." -> "Python File" and give it a name like "sla_protected_objects". 
Then paste this into the script window to get started
```
# RSET Python Lab script to get SLA info based on the SLA name.
import urllib3
import rubrik_cdm
import pprint


def main():
    urllib3.disable_warnings()
    rubrik = rubrik_cdm.Connect()
    pp = pprint.PrettyPrinter(indent=2)


if __name__ == '__main__':
    main()
```
![Screen Shot 2022-07-01 at 2 50 32 PM](https://user-images.githubusercontent.com/26155271/176966343-5fa8d114-dbaf-4049-bb6b-aefbfd80576b.png)

For this example let's get a run time argument, the name of an SLA Domain Policy. There are a number of ways to get the script arguments. One way is to use argparse. We import it, create a parser object, then use that to get the arguments. Make sure this code is correctly indented for the main function. The IDE will show you if you are doing it wrong. 
```
    parser = argparse.ArgumentParser()
    parser.add_argument('-s', '--sla_name', dest='sla_name', required=True, help="The name of the SLA to check how "
                                                                                 "many objects it is protecting",
                        default=None)

    args = parser.parse_args()
```
Also, we will need to import argparse so add that to the import section at the top of the script
```
import argparse
```
Now add a get on the cluster to use what we did in the REPL. As long as you have the OS environment variable set for the cluster address and API token the auth will be automatic from the rubrik_cmd package. 

```
    print("The input arg is : {}".format(args.sla_name))
    cluster_info = rubrik.get('v1', '/cluster/me')
    print("Connected to the {0} cluster. The full cluster json is :".format(cluster_info['name']))
    pp.pprint(cluster_info)
```

Now you can run this script (green arrow or Run menu)
![Screen Shot 2022-07-01 at 3 25 05 PM](https://user-images.githubusercontent.com/26155271/176969856-aaf68967-4c5b-431a-98df-3c002ea574e3.png)
We set the argument we added to be required but did not pass one to the script. Argparse generated an automatic usage statement to warn about the missing argument. The usage can be customized. This is just the basic auto generated usage. 

To pass in the SLA Name, go the Pycharm menu then "Run" -> "Edit Configurations...". Then for each script being run show on the left there is a "Script path:" and a "Parameters:" field on the right. In the "Parameters:" field add "-s <the name of a SLA on the cdm>".
  
![Screen Shot 2022-07-01 at 3 39 33 PM](https://user-images.githubusercontent.com/26155271/176970906-d8778e1e-a771-446b-80d4-8ff15436f119.png)

 Run the script again.

![Screen Shot 2022-07-01 at 3 41 30 PM](https://user-images.githubusercontent.com/26155271/176971039-f1392ce0-f0e1-4fea-bf93-b475422dd2b7.png)

If everything worked, we now have the Rubrik connection and have parsed the script argument. Now lets get the SLA info.
Put together the statements to get the sla info and print it out. 
  
Or scroll down to get the code
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR>
<BR> 
```
    sla_info = rubrik.get('v2', '/sla_domain?name={}'.format(args.sla_name))
    print("SLA details for {0}: ".format(args.sla_name))
    pp.pprint(sla_info)
```
![Screen Shot 2022-07-01 at 3 50 23 PM](https://user-images.githubusercontent.com/26155271/176971680-dab554ee-d903-4e58-9172-97dd72c3b52d.png)
        
        
 ## Resources
#### Python Org
  
https://www.python.org/
      

Python Docs
        
https://docs.python.org/3/
        
Python Package Index
        
https://pypi.org/
        
Style Guide
        
https://peps.python.org/pep-0008/
        
