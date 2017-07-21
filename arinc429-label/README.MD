# ARINC429 message label modeling in DFDL

This project contains various files related to an effort to model ARINC429 messages in DFDL. More specifically, we are trying to model the label (least significant 8 bits) of the message.

Two main files are supplied:

  1. a DFDL schema: `localizer-deviation-label.dfdl.xsd`
  2. a binary file containing the example message: `localizer-deviation-label.bin`
  
The DFDL schema was tested using the [daffodil tool](https://opensource.ncsa.illinois.edu/confluence/display/DFDL/Getting+Daffodil), version 2.0.

This project was used to fine tune parsing and unparsing of the label field in ARINC429 messages.

## Parsing the message

To parse the localizer deviation message, run the following command in a command prompt where the daffodil executable is on the path:

`daffodil parse -s localizer-deviation-label.dfdl.xsd -o localizer-deviation-label.xml localizer-deviation-label.bin`

The above operation should yield the following `localizer-deviation-label.xml`:

```xml
<localizer-deviation>
  <label>123</label>
</localizer-deviation>
```

## Unparsing the message

To unparse the resulting XML infoset back to a binary file, run the following command:

`daffodil unparse -s localizer-deviation-label.dfdl.xsd -o localizer-deviation-label2.bin localizer-deviation-label.xml`

The above operation should yield the value `DE` in `localizer-deviation-label2.bin`.