# Nightmare Modules and CSVs

Those of you who have used Nightmare in the past may be familiar with **Nightmare Modules** \(.nmm\). Nightmare Modules outline the location and length of tables as well as the headers of rows and columns. We can take advantage of these to **create .csv files of various tables** to edit them in a more visually friendly manner.  
  
The two important pieces we will need are the [Modules themselves](http://www.feshrine.net/hacking/nightmare.php) and [NMM2CSV](https://feuniverse.us/t/nmm2csv-edit-tables-with-excel-instead-of-nightmare-updated-to-v1-0/1748).

## N2C - Making CSVs from Nightmare Modules

The N2C program will scan for all .nmm files in the folder \(and any lower folders\) and create .csv files from them. When running N2C, it will ask for a clean ROM so it can fill in the default table values. Once completed, you will have .csv files ready for editing in the spreadsheet program of your choice! This makes it much easier to navigate which values correspond to what and presents data "rows and columns" in a more human-friendly manner.

{% hint style="danger" %}
 **Be careful when running N2C!** 

N2C will create new files for every .nmm it encounters, even if the .csv already exists. This means it can potentially replace your edited .csv files with fresh ones**.** I recommend running N2C in a folder separate from your project folder.
{% endhint %}

## C2EA - Inserting CSVs in EA

Editing .csv files is great, but it would be meaningless without someway to transfer our edited data back into the ROM. This is where C2EA comes in. C2EA converts all .csv files it finds in the folder \(and lower folders\) and converts them into .event files so EA can insert them. It also creates a Master Table Installer file so that you only need to \#include one file instead of manually needing to \#include every table you edit.



## Editing and Creating .nmm files

