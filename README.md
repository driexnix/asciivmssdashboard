# ASCii VMSS Dashboard v1.2
Dashboard to show and configure Azure VM Scale Set status and properties

![Image of ASCii VMSS Dashboard](https://raw.githubusercontent.com/msleal/msleal.github.com/master/asciidash-v1-2.png)


## Installation
  1. Install Python 2.x or 3.x.
  2. Install the azurerm REST wrappers for Microsoft Azure: pip install azurerm.
  3. Install [Pyunicurses](https://sourceforge.net/projects/pyunicurses/).
  4. On Windows, Install [pdcurses](http://pdcurses.sourceforge.net/). I have used the: pdc34dllw.zip file. See 'PDCURSES' bellow...
  5. Clone this repo locally.
  6. Register an Azure application, create a service principal and get your tenant id. See "Using ASCiiVMSSDashboard".
  7. Put in values for your application, along with your resource group name, and VM Scale Set name in vmssconfig.json.
  8. Run (on Linux): ./console.py or (on Windows): python console.py.
  9. To Exit the Console, just hit: Ctrl+C or use the command: 'quit' (for a 'clean' exit, we will wait for the update threads to finish).
In the Windows Platform you can just use the command: 'quit' for now (See "CAVEATS" bellow)...

## WATCH THE CONSOLE IN ACTION:
Subtitle/Captions should be enabled by default, but if not, enable them to follow the action (English and Portuguese BR available).

[ASCiiVMSSDashboard (v1.0) - Screencast](https://www.youtube.com/watch?v=MomiZ9rU9NU)
Don't forget to subscribe to the channel for updates...

Enjoy some code and loud music!

## FEATURES:
Multiple pages for visualization of all your Virtual Machines (each page shows 100 VM's).
This is the General Info Page:

![Image of ASCii VMSS Dashboard General Info](https://raw.githubusercontent.com/msleal/msleal.github.com/master/general.png)

Here you can see a screenshot showing the first page:

![Image of ASCii VMSS Dashboard Multiple Pages](https://raw.githubusercontent.com/msleal/msleal.github.com/master/virtualmachines.png)

Next, we can see the second page:

![Image of ASCii VMSS Dashboard Second page](https://raw.githubusercontent.com/msleal/msleal.github.com/master/virtualmachines2.png)

The console will show you your subscription/region usage and limits:

![Image of ASCii VMSS Dashboard Usage](https://raw.githubusercontent.com/msleal/msleal.github.com/master/usage.png)

The ASCiiVMSSDashboard can run on Python 2.x or 3.x versions, and you will see the version it is running at the top title:
![Image of ASCii VMSS Dashboard Python Version](https://raw.githubusercontent.com/msleal/msleal.github.com/master/version.png)

In case of any errors, you will be able to see the messages on the LOG Window, and also as a specific message in the main window:
![Image of ASCii VMSS Dashboard Error Message](https://raw.githubusercontent.com/msleal/msleal.github.com/master/error.png)

###TIP #1: 
To not create the .pyc files, I use the following (on Linux): export PYTHONDONTWRITEBYTECODE=1; ./console.py.

###TIP #2:
I have used the console with no issues using a refresh interval of 60 seconds. If you use a more 'agressive'
update interval, keep one eye at the last-update registered at the top-left of the dashboard window, to see if the console 
is stil running.  If you notice it stopped, it should have generated an 'error.log' in the current directory. 
Look into it, if it has information about AZURE API 'throttling", you will need to restart it (with a bigger inteval)...
 
###TIP #3:
As we wait for the threads to finish as you hit 'Ctrl+C' or 'quit' (to exit), the time you will wait to get your prompt
back will be proportional to you refresh interval (e.g.: max='INTERVAL'). You can change the update interval in the 
'vmssconfig.json' file.

## ASCiiVMSSDashboard Commands
  To issue commands for the Azure Resource Manager API to add and/or delete virtual machines from the Scale Set,
you just need to type ':'. After that, the cursor will appear at (PROMPT window), and you will be able to enter commands.
To see a help window just type ':' (to activate the command PROMPT), and 'help' again to hide it.

REMEMBER: To activate the prompt window, type: ':'

Commands (v1.2):
- add vm 'nr': Use this command to add virtual machines to your VMSS deployment.
- del vm 'nr': Use this command to delete virtual machines to your VMSS deployment.
- select vm 'nr': Use this command to select a specific virtual machine on your VMSS deployment and see detailed info.
You can select any Virtual Machine, and get specific details about it:
![Image of ASCii VMSS Dashboard VM Details](https://raw.githubusercontent.com/msleal/msleal.github.com/master/vmdetails.png)

- deselect: Use this command to clear the selection of any specific virtual machine (and hide the VM details window).
- rg 'resourcegroupname' vmss 'vmscalesetname': Use this command to switch the visualization to another VM Scale Set.
- quit 'or' exit: Use this command to exit the console (Any platform).
- Ctrl+c: Use this key combination to exit the ASCiiVMSSDashboard (Not working on windows for now).
- help: Use this command to get help about the dashboard commands (inside the ASCiiVMSSDashboard).

## CAVEATS
- The 'stdscr.nodelay(1)' seems not to be multiplatform (at least does not work on Windows), and we are using it 
as a non-block function when reading user commands. For now, use the command 'quit' or 'exit' to end the dashboard on Windows. 
I'm looking for an alternative non-block call to use on windows and fix this.
- It would be nice to have any feedback of this program running on MacOS or any other platform... 

##PDCURSES
- If you have problems installing pdcurses on windows (not able to load uniCurses: import error), you can just add the DLL directly
on the current directory of the ASCiiVMSSDashboard installation. To run the ASCiiVMSSDashboard on Windows, I have tested it 
just cloning the repo on Windows 10, and copying the 'pdcurses.dll' file to the cloned folder, and it runs without any issues (you
still needs to have the UniCurses installed, but you have the Windows Installer link at the top of this README file). 

## Using ASCiiVMSSDashboard 
To use this app (and in general to access Azure Resource Manager from a program without going through 2 factor authentication), 
you need to register your application with Azure and create a "Service Principal" (an application equivalent of a user). 
Once you've done this you'll have 3 pieces of information: A 'tenant ID', an 'application ID', and an 'application secret'.
You will use these to populate the 'vmssconfig.json' file. 

For more information on how to get this information go here: [Authenticating a service principal with Azure Resource Manager][service-principle].
See also: [Azure Resource Manager REST calls from Python][python-auth].

## Example VMSS ARM Template:
[Ubuntu Linux VM Scale Set integrated with Azure autoscale][arm-template].

[service-principle]: https://azure.microsoft.com/en-us/documentation/articles/resource-group-authenticate-service-principal/
[python-auth]: https://msftstack.wordpress.com/2016/01/05/azure-resource-manager-authentication-with-python
[arm-template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-autoscale
