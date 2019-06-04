# AP_ItemCompare
Simple XML tool application for diffing TIMS SAAIF files

## Overview
AP_ItemCompare is a command line tool that downloads item files from Gitlab and compares the files used during the initial import with the current TIMS version.

The files that are compared are:
* Item xml (e.g. item-200-2323.xml or stim-200-43423.xml)
* metadata.xml
* qrx file (if applicable)
* gax file (if applicable)
* eax file (if applicable)


## Setup

### Dependencies

#### Java 8
Download and install the Java 8 SE Runtime environment for your operating system:
 [Java SE Runtime](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)


#### Environment variables
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

Work with the development team to populate the missing values

### Setup project files
1. Create a local directories configured as the `TIMS_COMPARE_REPORT_DIR` and `TIMS_COMPARE_TEMP_DIR` environment variables
1. Download most recent Jar file of the ap-item-compare application to the `TIMS_COMPARE_REPORT_DIR`. [Current Download Url](https://airdev.jfrog.io/airdev/libs-releases-local/org/opentestsystem/ap/ap-item-compare/0.1.0/ap-item-compare-0.1.0.jar)
1. Copy [application.yml](https://github.com/SmarterApp/AP_ItemCompare/blob/develop/application.yml) to the `TIMS_COMPARE_REPORT_DIR`

### Item Id Configuration
Prior to running the compare application, the ids of the items that will be compared should be saved in the `compare-ids.txt` file located on the `TIMS_COMPARE_REPORT_DIR` directory. Example values:

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
 1. Execute `java -jar ap-item-compare-0.1.0.jar`. Note: the specific version number might change.
 
## Results
The Item Compare Application produces two reports. During the execution of the Item Compare Application a new directory will be created in `TIMS_COMPARE_REPORT_DIR` following this format `tims-compare-<date-timestamp>`. 
This directory will contain both reports.  

### Low level Item report
One file is created for each valid item number entered in `compare-ids.txt`. e.g. `18786.csv`

#### Columns
* `ItemId`: The id of the item
* `ItemType`: The type of the item
* `FileName`: The item filename where the difference was found 
* `DifferenceType`: An enumeration of the type of difference
* `ImportElementName`: Element name where the difference was found
* `ImportAttributes`: Element attributed where the difference was found
* `ImportValue`: Value on the file used during the initial import
* `TimsValue`: Value on the current file stored in TIMS
* `ImportXPath`: XPath value that can be used to programmatically retrieve Xml values from the import file

#### Sample low level item report 
```csv
"ItemId","ItemType","FileName","DifferenceType","ImportElementName","ImportAttributes","ImportValue","TimsValue","ImportXPath"
"18786","mc","item-200-18786.xml","CHILD_LOOKUP","attrib","attid='itm_item_desc'","attrib","","/itemrelease[1]/item[1]/attriblist[1]/attrib[3]"
"18786","mc","item-200-18786.xml","ATTR_VALUE","attachment","","BRF","ASL","/itemrelease[1]/item[1]/content[1]/attachmentlist[1]/attachment[1]/@type"
"18786","mc","item-200-18786.xml","CHILD_LOOKUP","","","","source","/itemrelease[1]/item[1]/content[1]/attachmentlist[1]/attachment[1]"
"18786","mc","item-200-18786.xml","ATTR_VALUE","attachment","","aslfile1","699631731","/itemrelease[1]/item[1]/content[1]/attachmentlist[1]/attachment[3]/@id"
"18786","mc","metadata.xml","TEXT_VALUE","SecurityStatus","","Secure","secure","/metadata[1]/smarterAppMetadata[1]/SecurityStatus[1]/text()[1]"
"18786","mc","metadata.xml","TEXT_VALUE","Version","","16","16.0","/metadata[1]/smarterAppMetadata[1]/Version[1]/text()[1]"
"18786","mc","metadata.xml","TEXT_VALUE","BrailleType","","BRF_EBAE","BRF","/metadata[1]/smarterAppMetadata[1]/BrailleType[1]/text()[1]"
"18786","mc","metadata.xml","CHILD_LOOKUP","EnemyItem","","#text","","/metadata[1]/smarterAppMetadata[1]/EnemyItem[1]/text()[1]"
"18786","mc","metadata.xml","TEXT_VALUE","Status","","Operational","Released","/metadata[1]/smarterAppMetadata[1]/Status[1]/text()[1]"
"18786","mc","metadata.xml","CHILD_LOOKUP","IrtDimension","","IrtDimension","","/metadata[1]/smarterAppMetadata[1]/IrtDimension[1]"
```
