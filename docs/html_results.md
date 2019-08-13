# HTML Comparison Report 
The HTML Comparison report is slightly different than the XML report as it is focused on the fields that have CDATA HTML within them.

HTML can have UTF-8 characters that do not display correctly in Excel unless specific steps are followed.  Those steps are documented [here](import_utf8_excel.md).

## HTML Summary Report

The summary report provides counts for each item on the number of errors that are found.  Each row in the summary has counts for a single item.  The columns will grow as more comparison features are added.

| Column Name | Description |
| ----- | ----- |
| ITEM_ID | The item id for the row |
| ITEM_TYPE | The item type |
| ERRORS | These are errors that occured during the processing which may have resulted in comparisons not being able to be compared |
| IMPORT_VALUE_NOT_FOUND | The value was in the current XML but could not be found in the import.zip |
| TIMS_VALUE_NOT_FOUND | The value was in the import XML but could not be found in the TIMS produced XML |
| TEXT_DIFFERENCE | The number of text differences within the HTML that were found for the item |
| STYLE_DIFFERENCE |  The number of style differences within the HTML that were found for the item.  Any style differenct (bold, underline, italics, etc.) will be included in this count.

## HTML Detail Report
In addition to the summary report a detailed report is created for each item with more detail than the summary report.

| Columnb Name | Description |
| ---- | ---- |
| DIFFERENCE_TYPE | This will currently be TEXT or STYLE.  Currently the codes are described in the Summary report section. |
| FILENAME | The file used for the report. |
| LOCATION | This defines where content the difference was found.  For example, differences found in the stem/prompt area are identified as Langauge_Prompt with the Language being replaced with English or Spanish depending where the difference was found. |
| IMPORT_VALUE | The HTML content with the difference found in the Smarter source data. |
| TIMS_VALUE | The HTML content with the difference found in the TIMS produced data. |

**Note** - HTML text differences have the <OLD> and <NEW> tags surrounding the HTML indicating what is new or old in the differences.