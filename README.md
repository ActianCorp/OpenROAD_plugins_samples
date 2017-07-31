# Sample OpenROAD Script Editor Plugins

## Get Started

This assumes a local Ingres DBMS, a new database named `plugins` will be used.

    createdb plugins
    backupapps /reload /dbname plugins
    w4gldev makeimage plugins scripted_plugins scripted_plugins.plb -f -e -w -Tall

NOTE this loads all applications found in the current directory.

Now launch Workbench with new plugins active:

    w4gldev runimage workbnch.img

## Making changes

Once changes have been made to the plugins, export via:

    backupapps /unload /dbname plugins /lis app_list.txt

NOTE the list file argument limits the applications that are exported.

## Description of the scripted_plugins application

Currently this plugin application implements the following predefined script editor hooks using
3 4GL procedures with predefined (invariant) names:

1) 	PI_Initialize():
The name of the 4GL procedure that is run when the script editor is started.  It is responsible
for implementing 3 plugin procedures and frames:

	a)  PI_lowercasestring():
	    A procedure that converts the currently selected script text to lowercase characters.
	    
	b)  PI_uppercasestring():
	    A procedure that converts the currently selected script text to uppercase characters.
	    
	c)  selectindentation():
	    A frame that allows the user to specify the number of spaces associated with the
	    Tab key and whether Tabs themselves consist of Space characters.

The PI_Initialize() 4GL procedure adds the above named procedures into the predefined "plugin_menuitems"
MenuGroup. The PI_Initialize() script then changes the MenuGroup's label to "Options" after adding the
procedures and frames.

The PI_Initialize procedure also enables an OptionField with the predefined name "plugin_options", adds the
PI_lowercasestring() and PI_uppercasestring() procedures to the OptionField and changes the OptionField's
text label to "Options:"

2)	PI_ScriptAdded() -
The name of the procedure that is run after the 4GL script itself is initially loaded into the script
editor. It currently writes the script's text to a file (c:\temp\script.before) for later diff'ing.

3)	PI_ScriptBeforeSave() -
The name of the procedure that is run prior to the script being saved.  This procedure currently performs
a "windy" on the script and displays a window showing the changes made since the script was initially
loaded into the script editor.  The PI_ScriptBeforeSave() script supports a return value (integer) which
when set to 0 aborts the process of saving the script.  Currently the return value returns a default value of 1.
