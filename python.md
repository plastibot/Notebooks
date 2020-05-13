# Virtual Environments 101

## Multiple versions of python in one system

To be able to have many versions of python installed use Pyenv. Instal Pyenv as follows. Refer to this link for more information
about pyenv. 

https://realpython.com/intro-to-pyenv/

#### 1. Install dependencies
````zsh
$ brew install openssl readline sqlite3 xz zlib
````

#### 2. Install Pyenv
````zsh
curl https://pyenv.run | bash
````

#### 3. Select the right version of python to use
Once installed, use Pyenv to select the version you want to use for your virtual environment. For instance if you want to use 
python 3.6.9 enter the following:

````zsh
$ pyenv global 3.6.9
````

Verify 3.6.9 is active. the active version shows an "*"
````zsh
$ pyenv versions
  system
* 3.6.9 (set by /Users/freeman/.pyenv/version)
````


## Setup the virtual environment

There are different ways to create a virtual environment. venv (#4 below is the recommended way).

#### 1. Conda
This is provided by the Anaconda installation of python. Used mostly for Data Science it is not recommended for code development.

#### 2. virtualenv 
This is the oldest method and does not come with python by default. It needs to be installed from Pypi. It is deprecated now.

#### 3. Pyvenv
It ships with Python since version 3.4 however it's been deprecated since version 3.6

#### 4. venv
It ships with Python since version 3.6 and it is the recommended way to create virtual environments.

first create your project folder
````zsh
$ mkdir project
$cd project
````

then proceed to create your virtual environment within the project.
````zsh
$ python -m venv .venv
````

