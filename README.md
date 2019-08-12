# AP_ItemCompare
Simple XML tool application for diffing TIMS SAAIF files

## Overview
AP_ItemCompare is a command line tool that downloads item files from Gitlab and compares the files used during the initial import with the current TIMS version.

There are multiple ways to compare items.  The currently supported compare modes are:
* `xml` - This compare mode examines the import and TIMS versions of multiple files for XML structural differences.
  
  The files that are compared are:
  * Item xml (e.g. item-200-2323.xml or stim-200-43423.xml)
  * metadata.xml
  * qrx file (if applicable)
  * gax file (if applicable)
  * eax file (if applicable)
  
  Differences are reported as [XmlUnit Difference Types](docs/difference-types.md)
* `html` - Coming soon

By default `all` compare modes are run. See [Run Compare Application](#run-compare-application) for details.

## Setup

### Dependencies

#### Java 8
Download and install the Java 8 SE Runtime environment for your operating system:
 [Java SE Runtime](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)

### Setup project files
Create local Report directory. This will be configured as the `TIMS_COMPARE_REPORT_DIR` environment variable.

A simple directory structure like this is recommended:
   ```
   ~/ItemCompareReports
   ```
1. Go to the AP_ItemCompare [Releases](https://github.com/SmarterApp/AP_ItemCompare/releases) page.
1. From the most recent release, download the following files to the Report directory created earlier:
   1. Application jar file. File name will contain the most recent version number.
   1. application.yml
   1. runCompareProd.bat
   1. runCompareProd.sh
1. Rename the Jar file to `ap-item-compare.jar`
1. Create an empty text file named `compare-ids.txt` in the Report directory
1. If your operating system is MacOs 
   1. Open a terminal window
   1. Navigate to the Report directory
   1. Run the following command `chmod +x runCompareProd.sh` to make the shell script executable

### Environment variables
Setup the following environment variables on your local computer

```
TIMS_COMPARE_GITLAB_SOURCE_BANK_ACCESS_TOKEN=
TIMS_COMPARE_GITLAB_ACCESS_TOKEN=
TIMS_COMPARE_GITLAB_GROUP=
TIMS_COMPARE_GITLAB_HOST=
TIMS_COMPARE_GITLAB_PASSWORD=
TIMS_COMPARE_GITLAB_USER=
TIMS_COMPARE_REPORT_DIR=~/ItemCompareReports
```

One may need to work with the team managing Gitlab to populate the missing values.

#### Environment Variables Description
To use the item compare tool users will need to have credentials to access the Gitlab itembank.  In addition the user will need to be able to get the secret key created.  The team managing Gitlab should be able to provide this information.

Environment Variable | Description 
|-------|------|
| `TIMS_COMPARE_GITLAB_SOURCE_BANK_ACCESS_TOKEN`| The user's Gitlab token.  Documentation [here](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html). |
| `TIMS_COMPARE_GITLAB_ACCESS_TOKEN` | This is the same token as the `TIMS_COMPARE_GITLAB_SOURCE_BANK_ACCESS_TOKEN`. |
| `TIMS_COMPARE_GITLAB_GROUP` | This is the Gitlab group containing the items.  For example, TIMS items in production are in `iat-prod`|
| `TIMS_COMPARE_GITLAB_HOST` | The host for the Gitlab itembank.  For example, `https://itembank.smarterbalanced.org/` |
| `TIMS_COMPARE_GITLAB_PASSWORD` | The user's password.  This is in plain text hence why it is saved in environment variables.
| `TIMS_COMPARE_GITLAB_USER` | The user's Gitlab username |
| `TIMS_COMPARE_REPORT_DIR` | The output folder containing the compare reports | 


### Item Id Configuration
Prior to running the compare application, the ids of the items that will be compared should be saved in the `compare-ids.txt` file. Example values:

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
 
 ### Run Compare application
 To run the compare application
 1. Open a terminal window
 1. Navigate to the `TIMS_COMPARE_REPORT_DIR` directory
 1. Based on your operating system run one of the following commands:
    * On MacOs
      * run `./runCompareProd.sh` to compare items in the production environment
    * On Windows
      * run `runCompareProd.bat` to compare items in the production environment
---
**NOTE:** 

To run in only specific compare modes, execute the script with an additional argument:<br>
`--compare.comparers=xml,html`<br>
If not provided, `all` compare modes will be executed.

---
  
## Results
The Item Compare Application produces two types of reports:
* Low level Item Report (Example: `18786.csv`)
* High-level Summary Report (Named `summary.csv`)

During the execution of the Item Compare Application a new directory will be created in `TIMS_COMPARE_REPORT_DIR` following this format `tims-compare-<date-timestamp>`. 
This directory will contain all reports.

### Low level Item Report
One file is created for each valid item number entered in `compare-ids.txt`. e.g. `18786.csv`

#### Columns
* `DIFFERENCE_TYPE`: The type of difference.  Valid values are different for difference compare modes.
* `FILENAME`: The item filename where the difference was found.
* `LOCATION`: Location within the file where the difference was found.
* `IMPORT_VALUE`: Value on the file used during the initial import.
* `TIMS_VALUE`: Value on the current file stored in TIMS

#### Sample low level item report 
```csv
DIFFERENCE_TYPE,FILENAME,LOCATION,IMPORT_VALUE,TIMS_VALUE
ATTR_VALUE,item-200-183467.xml,/itemrelease[1]/item[1]/@bankkey,187,200
ATTR_VALUE,item-200-183467.xml,/itemrelease[1]/item[1]/@id,3467,183467
CHILD_LOOKUP,item-200-183467.xml,/itemrelease[1]/item[1]/attriblist[1]/attrib[7]/desc[1]/text()[1],4,NOT FOUND
CHILD_LOOKUP,item-200-183467.xml,/itemrelease[1]/item[1]/attriblist[1]/attrib[9]/desc[1]/text()[1],8 [9.16],NOT FOUND
CHILD_LOOKUP,item-200-183467.xml,/itemrelease[1]/item[1]/attriblist[1]/attrib[3],attrib,NOT FOUND
CHILD_LOOKUP,item-200-183467.xml,/itemrelease[1]/item[1]/attriblist[1]/attrib[4],attrib,NOT FOUND
CHILD_LOOKUP,item-200-183467.xml,/itemrelease[1]/item[1]/attriblist[1]/attrib[5],attrib,NOT FOUND
CHILD_LOOKUP,item-200-183467.xml,/itemrelease[1]/item[1]/attriblist[1]/attrib[12],attrib,NOT FOUND
CHILD_LOOKUP,item-200-183467.xml,/itemrelease[1]/item[1]/attriblist[1]/attrib[13],attrib,NOT FOUND
CHILD_LOOKUP,item-200-183467.xml,/itemrelease[1]/item[1]/attriblist[1]/attrib[14],attrib,NOT FOUND
ATTR_VALUE,item-200-183467.xml,/itemrelease[1]/item[1]/tutorial[1]/@bankkey,187,200
ATTR_VALUE,item-200-183467.xml,/itemrelease[1]/item[1]/tutorial[1]/@id,1074,181074
ATTR_VALUE,item-200-183467.xml,/itemrelease[1]/item[1]/resourceslist[1]/resource[1]/@bankkey,187,200
ATTR_VALUE,item-200-183467.xml,/itemrelease[1]/item[1]/resourceslist[1]/resource[1]/@id,3468,600183467
ATTR_VALUE,item-200-183467.xml,/itemrelease[1]/item[1]/MachineRubric[1]/@filename,Item_3467_v0.qrx,Item_183467_v11.qrx
ATTR_VALUE,item-200-183467.xml,/itemrelease[1]/item[1]/RendererSpec[1]/@filename,Item_3467_v0.eax,Item_183467_v11.eax
```

### High-level Summary Report
One file is created for the entire compare execution. This report will report the number of each DIFFERENCE_TYPE for each source item. 
The high-level report file name is always `summary.csv`

#### Columns
The columns of the summary report are based upon which compare modes are used, since each compare mode contains separate difference types.<br>
The common columns regardless of compare modes are:
* `ITEM_ID`: The id of the item for the row
* `ITEM_TYPE`: The item type (Example `eq`|`mc`|`mi`|etc)
* `ERRORS`: The count of errors encountered during downloading/parsing/processing the item<br>
  (Error details can be found in the execution logs as well as the low-level report.)
* Following columns are all allowed DIFFERENCE_TYPE values based upon the compare modes with associated counts per item.

#### Sample high level item report 
```csv
ITEM_ID,ITEM_TYPE,ERRORS,IMPORT_FILE_NOT_FOUND,TIMS_FILE_NOT_FOUND,XML_VERSION,XML_STANDALONE,XML_ENCODING,HAS_DOCTYPE_DECLARATION,DOCTYPE_NAME,DOCTYPE_PUBLIC_ID,DOCTYPE_SYSTEM_ID,SCHEMA_LOCATION,NO_NAMESPACE_SCHEMA_LOCATION,NODE_TYPE,NAMESPACE_PREFIX,NAMESPACE_URI,TEXT_VALUE,PROCESSING_INSTRUCTION_TARGET,PROCESSING_INSTRUCTION_DATA,ELEMENT_TAG_NAME,ATTR_VALUE_EXPLICITLY_SPECIFIED,ELEMENT_NUM_ATTRIBUTES,ATTR_VALUE,CHILD_NODELIST_LENGTH,CHILD_NODELIST_SEQUENCE,CHILD_LOOKUP,ATTR_NAME_LOOKUP
987293874,UNKNOWN,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
182799,sa,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,9,0,0,0,0,0,29,0,0,29,8
8036,gi,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,3,0,0,0,0,0,24,0,0,25,6

```
### Additional Logging
Additional application logging is written to a file named `ap-compare.log`
