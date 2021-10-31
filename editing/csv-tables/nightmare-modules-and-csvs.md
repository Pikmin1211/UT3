# N2C and C2EA

Those of you who have used Nightmare in the past may be familiar with **Nightmare Modules** (.nmm). Nightmare Modules outline the location and length of tables as well as the headers of rows and columns. We can take advantage of these to **create .csv files of various tables** to edit them in a more visually friendly manner.

The two important pieces we will need are the [Modules themselves](http://www.feshrine.net/hacking/nightmare.php) and [NMM2CSV](https://feuniverse.us/t/nmm2csv-edit-tables-with-excel-instead-of-nightmare-updated-to-v1-0/1748).

## N2C - Making CSVs from Nightmare Modules

The N2C program will scan for all .nmm files in the folder (and any lower folders) and create .csv files from them. When running N2C, it will ask for a clean ROM so it can fill in the default table values. Once completed, you will have .csv files ready for editing in the spreadsheet program of your choice! This makes it much easier to navigate which values correspond to what and presents data "rows and columns" in a more human-friendly manner.

{% hint style="danger" %}
**Be careful when running N2C!**

N2C will create new files for every .nmm it encounters, even if the .csv already exists. This means it can potentially replace your edited .csv files with fresh ones. I recommend running N2C in a folder separate from your project folder.
{% endhint %}

## C2EA - Inserting CSVs in EA

Editing .csv files is great, but it would be meaningless without someway to transfer our edited data back into the ROM. This is where C2EA comes in. C2EA converts all .csv files it finds in the folder (and lower folders) and converts them into .event files so EA can insert them. It also creates a Master Table Installer file so that you only need to #include one file instead of manually needing to #include every table you edit.

## Editing and Creating .nmm files

Sometimes, you'll find that a field in your table is improperly labelled or sized. For fixing names you can just edit them in the .csv file, but fields and their sizes are determined using the .nmm file for the table. In these cases, or if you want to make a new table, you'll need to edit .nmm files. If something is incorrect with the fields in a .nmm file that you already have, knowing the structure of the file will let you fix it.

Much like .event files, **.nmm files are just plain text files**, and as such you can open them in any text editor. Their original use being with the program Nightmare, there's a good bit of information contained in .nmm files that we don't actually need to care about when using our .csv tables. We'll go over what you do and don't need to worry about as we go through.

.nmm files are split into sections, one for a header containing data on the overall table and one for each field in your .csv. **The first section of the file is always the header, and the rest are always fields.** The first line of the header is the version number of the .nmm and the second line is the name of the .nmm. N2C doesn't use either of these, but they can be useful just to give a description of the file to anyone in the future that's looking at it. The third line is the starting offset of the table. The fourth line is the number of entries and the fifth line is the size of each entry in bytes. These values can be in decimal or hexadecimal; if hexadecimal, you need to put `0x` on the front of the number to denote this.

The sixth line is the name of a text file. This file contains names for each index on the table; this gets used for names on the leftmost column of your .csv. The text file should be in the same folder as the .nmm file. Line seven is also the name of a text file, but it's irrelevant to a .csv so you can put `NULL` instead. At this point, your .nmm file should look something like this:

```
1
An Epic Module by Me
0xBEEF
256
16
EntryNames.txt
NULL
```

At this point, it's conventional to put an empty line between the end of this section and the start of the next one to help keep each section visually separate. Next, we begin the field sections. These start with a name on the first line, the number of bytes from the start of the entry on the second line, the length of the entry in bytes on the third line. The fourth line contains the **type**, which for our purposes is going to be either `NEHU` or `NEDU`. These stand for `Numeric editbox, hex unsigned` and `Numeric editbox, decimal unsigned` respectively, but for our purposes only the `hex` and `decimal` parts of these matter. When using Nightmare, the type here determines how it lets you edit the field and there are a number of ways in which you can specify. For a .csv, we only need these two as it tells N2C whether to write the value in hexadecimal or decimal. As a general rule, if a field contains an address, a bitfield, or an unknown value, it should be in hexadecimal. Otherwise, it should be in decimal; having numbers like this makes them easier to work with. Line five of a field section contains the name of a text file again, but once again this is not used by N2C so you can just make it `NULL`. As such, our .nmm file should now look similar to this:

```
1
An Epic Module by Me
0xBEEF
256
16
EntryNames.txt
NULL

The First Field
0
4
NEDU
NULL
```

Repeat field sections until you have a field for each value in the table; you don't need to put anything special at the end of the file. You don't strictly need a section for every byte of each entry; Nightmare edits things in-place, so if a section for part of the entry doesn't exist it wouldn't have a problem with it. However, a .csv file needs data for the entirety of each entry. As such, sections of an entry that you don't define fields for will get an `UNKNOWN` column in your .csv file between the columns where the data is located. If you want to avoid this, you can specify sections named `N/A` or similar for this data manually in the .nmm, and if at some later point you figure out what that data is used for you can easily change the name and have the section ready to go.
