# Item Compare XML Results

[Main Page](../README.md)

### Low level Item Report
One file is created for each valid item number entered in `compare-ids.txt`. e.g. `18786.csv`

#### Columns
* `DIFFERENCE_TYPE`: The type of difference.  Valid values are different for difference compare modes.
* `FILENAME`: The item filename where the difference was found.
* `LOCATION`: Location within the file where the difference was found.
* `IMPORT_VALUE`: Value on the file used during the initial import.
* `TIMS_VALUE`: Value on the current file stored in TIMS

#### Sample XML low level item report 
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
