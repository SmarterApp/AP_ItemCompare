# HTML Comparison Report 

[Main Page](../README.md)

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
| STYLE_DIFFERENCE |  The number of style differences within the HTML that were found for the item.  Any style difference (bold, underline, italics, etc.) will be included in this count.

## HTML Detail Report
In addition to the summary report a detailed report is created for each item with more detail than the summary report.

| Column Name | Description |
| ---- | ---- |
| DIFFERENCE_TYPE | This will currently be TEXT or STYLE.  Currently the codes are described in the Summary report section. |
| FILENAME | The file used for the report. |
| LOCATION | This defines where content the difference was found.  For example, differences found in the stem/prompt area are identified as Langauge_Prompt with the Language being replaced with English or Spanish depending where the difference was found. |
| IMPORT_VALUE | The HTML content with the difference found in the Smarter source data. |
| TIMS_VALUE | The HTML content with the difference found in the TIMS produced data. |

**Note** - HTML text differences have the `<OLD>` and `<NEW>` tags surrounding the HTML indicating what is new or old in the differences.

## Difference Type Definitions

### Text Differences

The text differences strips out the following prior to doing the diff:

* All HTML Tags
* Spaces are reduced to a single space.  If there is `"<p>This     is something</p>"` will be reduced to `"This is something"`

After those things are removed then the text is compared.  If the text is the same it will not be present in the report.  The tool marks the differences in the text.

| Markings | Descriptions | Example |
| -------- | ------------ | ------- |
| `<OLD>` | This will surround text that has been removed in the TIMS created HTML.  | `<OLD>Source</OLD> Value` |
| `<NEW>` | This will surround text that has been added in the TIMS created HTML | `<NEW>Test</NEW> Value` |

### Style Differences 

The tool attempts to find all style differences in the HTML.  It lines up the text and tags the best it can depending on the amount of changes made by the user since the content was added to TIMS.

* bolding
* italics
* super and subscript
* underline
* font changes

The snippets which the tool has identified as different will be surrounded by the changes.  One or all of the following can surround the snippet.

* "removed" means that the source had styling that the new does not
* "added" means that the new HTML has additional style not present in the source

| Code | Description | Example |
| ---- | ------ | ----- |
| BOLD | Represents a bolding was added or removed. | `<BOLD,ITALIC>italic</BOLD,ITALIC>` |
| ITALICS | Present when the italics were removed or added. | `<ITALIC>italic</ITALIC>` | 
| UNDERLINE | Present when the underline was removed or added. | `<UNDERLINE>text</UNDERLINE>` |
| FONT | Present when font size has changed. | `<FONT>some text...</FONT>` |
| SUPERSCRIPT | Present when the superscript was removed or added. | `<SUPERSCRIPT>other text</SUPERSCRIPT>` |
| SUBSCRIPT | Present when the superscript was removed or added. | `<SUBSCRIPT> some other text </SUBSCRIPT>` |
