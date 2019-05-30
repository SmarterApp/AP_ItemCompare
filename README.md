# AP_ItemCompare
Simple XML tool application for diffing TIMS SAAIF files

##Overview
AP_ItemCompare is a command line tool that downloads item files from Gitlab and compares the files used during the initial import with the current TIMS version.

The files that are compared are:
* Item xml (e.g. item-200-2323.xml or stim-200-43423.xml)
* metadata.xml
* qrx file (if applicable)
* gax file (if applicable)
* eax file (if applicable)


##Setup

###Clone project
Clone https://github.com/SmarterApp/AP_ItemCompare to a local directory

###Environment variables
Setup the following environment variables on your local computer

```
GITLAB_SOURCE_BANK_ACCESS_TOKEN=
GITLAB_ACCESS_TOKEN=
GITLAB_GROUP=
GITLAB_HOST=
GITLAB_PASSWORD=
GITLAB_USER=
TIMS_COMPARE_REPORT_DIR=~/ItemCompareReports
TIMS_COMPARE_TEMP_DIR=~/ItemBankCompare
```

Work with the development environment to populate the missing values

###Configuration
Prior to running the compare application, the ids of the items that will be compared should be saved in the `compare-ids.txt` file located on the root directory of the project. Example values:

```
45990
11065
2836
51576
18786
11417
25265
76749
 ```
 
 ###Run Compare application
 To run the compare application
 * Open a terminal window
 * Navigate to the root directory of the `AP_ItemMigration` project
 * execute `./gw bootRunCompare`