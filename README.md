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
