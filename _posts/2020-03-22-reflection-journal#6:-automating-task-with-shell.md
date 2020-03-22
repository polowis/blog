---
date: 2020-03-22 06:20:54
layout: post
title: "Reflection Journal#6: Automating task with shell"
subtitle:
description: week 7, semester 1. Automating task with shell
image: https://www.howtogeek.com/wp-content/uploads/2011/07/banner-011.png
optimized_image:
category:
tags:
    - shell
    - cli
    - reflection

author: polowis
paginate: false
---

Shell scripts are designed to be run on the command line. They are effectively good at customizing adminstrative tasks or performing automated tasks. Instead of manually installing packages or setting up my flask app. I think it would be better if I write a small shell script that going to handle it for me.

## The shell language
Shell is a language. It has its own unique way of writing. But we don't really have to follow its syntax. Why? because everytime we write something in our terminal/command line, we actually write valid shell scripts.

Let's take a look at this. If we want to activate our python virtual environment (for Linux & OSX only). 

```sh
source venv/bin/activate
```

Now if I create another file called ```python.sh``` and place the script above in there

```sh
#!/usr/bin

source venv/bin/activate
```

Then simply run
```
$ python.sh
```

It will activate the virtual environment same as what we usually do in our command-line. 

## Expanding

What can I write in there? Python package installation, activate virtual environment. But I don't need to write the entire shell script just to install python packages. Note that, on the previous [post](https://polowis.netlify.com/reflection-journal-5-vue-js-and-flask/), I wrote about how I use vue in my application. Because OSX and Windows process are quite different, so if I just copy and paste the script, it will not work on the Windows. Here is the script I wrote for OSX:

```sh
if [[ "$OSTYPE" == "darwin"* ]]; then
echo "Detecting OS: $OSTYPE";

# activate python virtual environment
python3 -m venv venv
source venv/bin/activate

# install packages from requirements.txt
pip install -r requirements.txt

# set the main entry point for flask app
export FLASK_APP=main.py

# migrate the database, assuming the database is setup correctly
flask db:migrate 
    
# install npm dependencies
npm install

#compile js assets
npm run dev
```

On Windows, it will be different. But we only look at OSX just for now. That will be a lot easier to deal with installation stuff in cases we have too many things need to install or setup. 

### What if python is not installed?

In our case, we assume that the user has already installed python. But what if they haven't installed it? The script will just break and the user will have no idea what happened.

Instead of letting it breaks (Like I usually do), I'm going to provide a friendly message to let them know something is wrong with the installation. 

In the ```setup.sh``` script, I'm going to check if python is installed and its version is above 3.6. Why 3.6? Just in case some python packages only support for python above 3.6. 

```sh
PYTHON_REF=$(source ./bin/python.sh) # change path if necessary
if [[ "$PYTHON_REF" == "NoPython" ]]; then
    echo "Python3.6+ is not installed."
    exit 1
fi

```

Now in the ```python.sh``` script, I will need to check if the Python is installed so that the ```setup.sh``` can use it.

```sh
# Set minimum required versions
PYTHON_MINIMUM_MAJOR=3
PYTHON_MINIMUM_MINOR=6

# Get python references
PYTHON3_REF=$(which python3 | grep "/python3")
PYTHON_REF=$(which python | grep "/python")

error_msg(){
    echo "NoPython"
}

python_ref(){
    local my_ref=$1
    echo $($my_ref -c 'import platform; major, minor, patch = platform.python_version_tuple(); print(major); print(minor);')
}

# Print success_msg/error_msg according to the provided minimum required versions
check_version(){
    local major=$1
    local minor=$2
    local python_ref=$3
    [[ $major -ge $PYTHON_MINIMUM_MAJOR && $minor -ge $PYTHON_MINIMUM_MINOR ]] && echo $python_ref || error_msg
}

# Logic
if [[ ! -z $PYTHON3_REF ]]; then
    version=($(python_ref python3))
    check_version ${version[0]} ${version[1]} $PYTHON3_REF
elif [[ ! -z $PYTHON_REF ]]; then
    # Didn't find python3, let's try python
    version=($(python_ref python))
    check_version ${version[0]} ${version[1]} $PYTHON_REF
else
    # Python is not installed at all
    error_msg
fi
```

### What if Nodejs is not installed?

Nodejs installation is a complicated process, because I'm using a Mac device, I will need to check if [brew](https://brew.sh) is installed, if not, install it. Then install nvm, and use nvm to install node.

If windows, I will need to install chocolatey (just like brew) then use it to install nvm and then node.

Here is example of installation:
```sh
isNVMInstalled(){
    if which nvm > /dev/null
    then
        echo "nvm is installed, skipping..."
        return 0
    else
        echo "nvm is not installed"
        return 1
    fi
}
RED='\033[0;31m'
WHITE='\033[00m'
WARNING='\033[93m'
SUCCESS='\033[92m'

installNVM(){
    brew install nvm
    source $(brew --prefix nvm)/nvm.sh
    echo "source $(brew --prefix nvm)/nvm.sh" >> ~/.profile
    if isNVMInstalled ; then
        source ./bin/node.sh
        installNodeWithNVM
    else 
        echo -e "${RED}Failed to install nvm ${WHITE}"
    fi
}
```

