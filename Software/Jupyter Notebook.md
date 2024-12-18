# Jyputer Notebook notes

## How to change starting directory:
* Open cmd (or Anaconda Prompt) and run ```jupyter notebook --generate-config```.
* This writes a file to ```C:\Users\username\.jupyter\jupyter_notebook_config.py```.
* Browse to the file location and open it in an Editor
* Search for the following line in the file: ```#c.NotebookApp.notebook_dir = ''```
* Replace by ```c.NotebookApp.notebook_dir = '/the/path/to/home/folder/'```
Make sure you use forward slashes in your path and use ```/home/user/``` instead of ```~/``` for your home directory, backslashes could be used if placed in double quotes (```"```) even if folder name contains spaces as such : ```"D:\yourUserName\Any Folder\More Folders\"```
* Remove the ```#``` at the beginning of the line to allow the line to execute