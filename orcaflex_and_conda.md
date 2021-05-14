Orcaflex has issues supporting python installed using anaconda or miniconda. Which is a shame because I do not know many people who do NOT use it.

So how to solve:

1. Install miniconda or anaconda

# Tell orcaflex where to find python

Now we need to tell orcaflex where to find python. This is done by adding a few entries to the windows registry. To do this, 
- copy the contents below into a text-file
- adjust*
- save as .reg 
- double-click to add to the registry.

Make the following changes to this file:

*Using environments?*
1. replace ```__env_name__``` with the name of your environment.
2. replace ```__python_miniconda_or_anaconda_folder__``` with the location where you installed python. Note that double \\ are needed. By default anaconda installs in the user-path which
means you'd have to replace by ```Users\\Ruben\\miniconda3``` or whatever your user-name is.
3. replace 3.8 and py38 with the python version that you are using

*Not using environments*
First of all consider to use environments. This is good practice. 
If not:

1. Replace ```c:\\__python_miniconda_or_anaconda_folder__\\envs\\__env_name__\\``` with the installation path of anaconda. In my case ```Users\\Ruben\\miniconda3``.
2. Replace ```"DisplayName"="__env_name__"``` with ```Base-env```
3. replace 3.8 and py38 with the python version that you are using


Template:
```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Python\PythonCore]


[HKEY_CURRENT_USER\Software\Python\PythonCore\3.8]
"DisplayName"="__env_name__"
"Version"="py38"
"SysVersion"="3.8"
"SysArchitecture"="64bit"

[HKEY_CURRENT_USER\Software\Python\PythonCore\3.8\InstallPath]
@="c:\\__python_miniconda_or_anaconda_folder__\\envs\\__env_name__"
"ExecutablePath"="c:\\__python_miniconda_or_anaconda_folder__\\envs\\__env_name__\\python.exe"
"WindowedExecutablePath"="c:\\__python_miniconda_or_anaconda_folder__\\envs\\__env_name__\\pythonw.exe"

[HKEY_CURRENT_USER\Software\Python\PythonCore\3.8\InstallPath\InstallGroup]
@="Python 3.8"

[HKEY_CURRENT_USER\Software\Python\PythonCore\3.8\Modules]
@=""

[HKEY_CURRENT_USER\Software\Python\PythonCore\3.8\PythonPath]
@="c:\\__python_miniconda_or_anaconda_folder__\\envs\\__env_name__\\Lib;c:\\__python_miniconda_or_anaconda_folder__\\envs\\__env_name__\\DLLs"
```

# Install orcaflex python api

Go the the "anaconda" or "miniconda" prompt.
Activate your environment, for example: ```activate my_env```
In the prompt, navigate to the orcaflex api folder and run the install script

# Not sure if this is still needed

Run orcaflex from within an activated conda environment.



