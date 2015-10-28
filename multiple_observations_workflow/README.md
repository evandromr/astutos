
# How to work with multiple X-ray data and Python

### Useful Python Standard Libraries

    **os**: allows manipulatio of paths and check if files and folders exists.  
    **glob**: allows the use of regular expression to find files (like '*.txt' to find all txt files).  
    **shutil**: easy comands to copy, move, and delete files.  
    **subprocess**: allow to call terminal comands inside python.  
    **sys**: I only use this with sys.exit() it interrupts the program.  

    You should browse the python documentation about these libraries to see what they can do ;)  

### Note about System variables 

    System variables are the variabes that you set with "setenv YOURVARIABLE somevalue" or "export YOURVARIABLE=somevalue" depending if your are using `c-shell` or `bash` in your terminal.  
    Some softwares in astronomy makes use of system variables to store values that are important to run the software.  

    The `subprocess.call()` function allow to call terminal comands but it does that using a sub-shell inside your python script that means that your system variables are not exported!  

    There are two ways to go around this:  
        
        1) define all the system variables you need before runing the scriptm, and use `shell=True` as a parameter in your call() function.  

        2) use `os.environ['YOURVARIABLE'] = somevalue` to set your system variables in the scope of the script.  

### Examples of functions that I use a lot

    - `glob.glob('\*.txt')` : returns a python list of all files in the current directory that ends with the string '.txt'. In this case '\*' means 'anything'.  
    - `os.path.exists('path/to/a/file_or_dir')`: checks if a file or directory exists, return python booleans True or False.  
      - can also be used as os.path.isdir() or os.path.isfile(). Check the python docs on os.path to have a look at the options.  
    - `os.chdir('other/dir')`: change to another directory inside the python script (like the command `cd` in the terminal).  
      - If you use this, don't forget to `chdir()` back to where you want to be. It's easy to get lost.  
    - `os.remove()`: delete a file (be carefull, it doesn't send to the thrash it's like `rm` from the terminal.  
    - `shutil.copyfile(src, dst)`: copy a file from 'src' to 'dst'. Check also `shutil.copy` it accepts folder as destinations.  
    - `shutil.move(src, dst)`: move a file to another location  
    
    - ** IMPORTANT **  
    - subprocess.call('command --and some parameters', shell=True)  
        - In this case the command is passed to the shell and uses your system variables but everything should be one single string: commands and parameters.  

    or  

    - subprocess.call('command', '--and', 'some', 'parameters')  
        - In this case the command is passed to a sub-shell and uses the system variables that you setup with `os.environ`: commands and parameters are separated by commas  
        - in some cases your parameter might need names like: ['command', 'filename=afile.txt', 'table=sometable.dat']. And usually everythin needs to be a string.  

### Semi interactive programs
    
    Some programs you may want to run needs you to type someting during the execution.  
    The only way I found to automatize those cases is if you already know everyting you want to type and in each order (including just hitting enter)  

    What I do in those cases is to write the commands you know in a file and pass the file to the program as an input:  

    A short example:  

        # I use three quotes because I'll use more than 1 line
        interactive_commands = """Firstparameter
        secondparameter

        thirdparameter
        {0}
        {1}
        sixparameter
        """.format(fourthparameter, fifthparameter)
        
        f = open('parameters.tmp', 'w')
        f.write(interactive_commands)
        f.close()
        subprocess.call('somecommand < parameters.tmp', shell=True)

    Notice: after the first line everything is *not* idented (this looks weir in a python script but otherwise the spaces could be read by the program)  
    blank lines are equivalent to hit enter in the prompt.  
    It should be possible to pass the string as it is, instead of writing to a file, but for me it didn't work. Maybe you can find a better way ;)  
    This syntax only works with `shell=True` as far as I could tell.  
