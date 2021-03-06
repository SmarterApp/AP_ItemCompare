# !!! IMPORTANT !!! - Archived
This project has been archived and is no longer actively maintained.  All code has been migrated to the [TIMS project](https://github.com/SmarterApp/TIMS).

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
1. Based on your operating system run one of the commands in the "Commands" section.

### Commands
Use the `*.bat` file for Windows and the `*.sh` command when running in a Linux type environment (i.e. Mac).  The examples below use the shell scripts.

* Default Operation
	* By default both the XML and HTML reports are run
	* This is currently limited to 400 or less items
	* Run `./runCompareProd.sh`
* XML Report
	* This runs only the XML Comparison Report
	* This does a XML element comparison ignoring all CDATA elements in the SAAIF data
	* Run `./runCompareProd.sh --compare.comparers=xml`
* HTML Report
	* This runs only the HTML Comparison Report
	* This runs comparisons against all the HTML (CDATA) elements within the SAAIF data
	* Run `./runCompareProd.sh --compare.comparers=html`
	
---
**NOTE:** 

To run in only specific compare modes, execute the script with an additional argument:<br>
`--compare.comparers=xml,html`<br>
If not provided, `all` compare modes will be executed.

---

## Known Errors
Due to external libraries this tool uses some errors will show up in the logs when running the report.  These are not critical errors but can be confusing to users.

* Invalid Token
	* `null [1:7] Error in expression. (Invalid token ";". Was expecting one of: <S>, <NUMBER>, "inherit", <IDENT>, <STRING>, "-", <PLUS>, <HASH>, <EMS>, <EXS>, <LENGTH_PX>, <LENGTH_CM>, <LENGTH_MM>, <LENGTH_IN>, <LENGTH_PT>, <LENGTH_PC>, <ANGLE_DEG>, <ANGLE_RAD>, <ANGLE_GRAD>, <TIME_MS>, <TIME_S>, <FREQ_HZ>, <FREQ_KHZ>, <RESOLUTION_DPI>, <RESOLUTION_DPCM>, <PERCENTAGE>, <DIMENSION>, <UNICODE_RANGE>, <URI>, <FUNCTION>, "progid:".)`
	* This error occurs when HTML is using invalid CSS.  When this happens the comparison is ignored as the element/css is invalid and cannot be processed.  All HTML should be valid HTML otherwise it will be ignored.

  
## Results
The Item Compare Application produces two types of reports:

* Low level Item Report (Example: `18786.csv`)
* High-level Summary Report (Named `summary.csv`)

The results are slightly different depending if you are running it in HTML or XML comparison mode.  

* [XML Report Definition](docs/xml_results.md)
* [HTML Report Definition](docs/html_results.md)

During the execution of the Item Compare Application a new directory will be created in `TIMS_COMPARE_REPORT_DIR` following this format `tims-compare-<date-timestamp>`. 
This directory will contain all reports.

### Additional Logging
Additional application logging is written to a file named `ap-compare.log`
