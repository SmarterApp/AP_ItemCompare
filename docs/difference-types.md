[XmlUnit](https://github.com/xmlunit/xmlunit) is the Xml comparison library used by the AP_ItemCompare application.

Below is a description of the most common types of differences encountered while comparing Item Xml data.

DifferenceType|Description of cause of difference
--------------|----------------------------------
TEXT_VALUE|The text value of an element is different
ATTR_VALUE|An attribute value from an element is different
ATTR_NAME_LOOKUP| An attribute from an element was not found
CHILD_LOOKUP|A child element was not found

For a full list of DifferenceType values please review the [XmlUnit library](https://github.com/xmlunit/xmlunit/blob/master/xmlunit-core/src/main/java/org/xmlunit/diff/ComparisonType.java)