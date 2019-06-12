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

### Setup project files
Create local directories. 
   * Report directory. This will be configured as the `TIMS_COMPARE_REPORT_DIR` environment variable.
   * Temporary directory. This will be configured as the `TIMS_COMPARE_TEMP_DIR` environment variable.

A simple directory structure like this is recommended:
   ```
   ~/ItemCompareReports
   ~/ItemCompareReports/temp
   ```
1. Go to the AP_ItemCompare [Releases](https://github.com/SmarterApp/AP_ItemCompare/releases) page.
1. From the most recent release, download the following files:
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
GITLAB_SOURCE_BANK_ACCESS_TOKEN=
GITLAB_ACCESS_TOKEN=
GITLAB_GROUP=
GITLAB_HOST=
GITLAB_PASSWORD=
GITLAB_USER=
TIMS_COMPARE_REPORT_DIR=~/ItemCompareReports
TIMS_COMPARE_TEMP_DIR=~/ItemCompareReports/temp
```

Work with the development team to populate the missing values

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
      * run `./runCompareUat.sh` to compare items in the uat environment
      * run `./runCompareDev.sh` to compare items in the development environment
    * On Windows
      * run `runCompareProd.bat` to compare items in the production environment
      * run `runCompareUat.bat` to compare items in the uat environment
      * run `runCompareDev.bat` to compare items in the development environment
   
## Results
The Item Compare Application produces two reports. During the execution of the Item Compare Application a new directory will be created in `TIMS_COMPARE_REPORT_DIR` following this format `tims-compare-<date-timestamp>`. 
This directory will contain both reports.  

### Low level Item report
One file is created for each valid item number entered in `compare-ids.txt`. e.g. `18786.csv`

#### Columns
* `ItemId`: The id of the item
* `ItemType`: The type of the item
* `FileName`: The item filename where the difference was found 
* `DifferenceType`: The type of difference. For more details please review: [XmlUnit Difference Types](docs/difference-types.md)
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

### High-level report
One file is created for the entire compare execution. This report will categorize the number of items having each particular difference to help identify common differences and patterns. 
The high-level report file name follows the following pattern `tims-compare-<timestamp>.csv`

#### Columns
* `DifferenceType`: The type of difference. For more details please review: [XmlUnit Difference Types](docs/difference-types.md)
* `ImportElementName`: Element name where the difference was found
* `ImportAttributes`: Element attributed where the difference was found
* `ImportValue`: Value on the file used during the initial import
* `TimsValue`: Value on the current file stored in TIMS
* `Count`: Occurrences of specific difference

#### Sample high level item report 
```csv
"DifferenceType","ImportElementName","ImportAttributes","ImportValue","TimsValue","Count"
"CHILD_LOOKUP","desc","","Tutorial_1","NOT_FOUND",1
"TEXT_VALUE","associatedpassage","","46","170046",1
"ATTR_VALUE","MachineRubric","","Item_11065_v6.qrx","Item_11065_v11.qrx",1
"ATTR_NAME_LOOKUP","samplelist","","minval","NOT_FOUND",2
"ATTR_VALUE","attachment","","item_45990_enu_brf_ebae_uncontracted_exl.brf","item_45990_enu_exl.brf",1
"CHILD_LOOKUP","NOT_FOUND","","NOT_FOUND","TertiaryClaim",8
"CHILD_LOOKUP","NOT_FOUND","","NOT_FOUND","source",10
"TEXT_VALUE","Version","","2","2.0",2
"ATTR_VALUE","attachment","","item_11417_enu_brf_ebae_uncontracted_nemeth_exn.brf","item_11417_enu_ecn.brf",1
"CHILD_LOOKUP","NOT_FOUND","","NOT_FOUND","LanguageFeatures",9
"CHILD_LOOKUP","NOT_FOUND","","NOT_FOUND","Claim2RevisionCategory",8
"CHILD_LOOKUP","NOT_FOUND","","NOT_FOUND","PrimaryClaim",8
"TEXT_VALUE","AssociatedStimulus","","46","170046",1
```
### Additional Logging
Additional application logging is written to a file named `ap-compare.log`
