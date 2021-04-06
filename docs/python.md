#

## Install a Virtual Environment

```python
#1. Install virtual env
pip install virtualenv

#2. Create a virual environment in the folder of your choice
cd your_folder
virtualenv name_of_your_virtual_environment

#3. Activate your virtual environment
source name_of_your_virtual_environment/bin/activate

#4 Now you can install pip packages in your virtual environment
pip install your_new_package

#5. When done, deactivate your virtual environment. You need to be in your_folder directory.
deactivate

#6. To delete your virtual environment just delete your_folder directory
rm -rf name_of_your_virtual_environment

#7 To list all your virtual environments
lsvirtualenv
```
Reference: https://python-guide-ru.readthedocs.io/en/latest/dev/virtualenvs.html




---



## List files in current directory

```python
import os

#Collect all files in present working directory
files = [file for file in os.listdir('.') if os.path.isfile(file)]

#Print all files
for file in files:
    print(file)
```

If you need to list files in a directory other than the present working directory:

```python
files = [file for file in os.path.isfile(os.path.join(another_directory, file))]
```


## Function to create a list to be used in 'WHERE IN' in an SQL query

```python

def format_list(pandas_series):
    #Takes in a pandas series, returns a string formated to use with 'WHERE IN' in our SQL query
    list = []
    for item in pandas_series:
        list.append(item)
    res = str(list).replace("[", "").replace("]", "")
    return res

```
