# ARINC429 message modeling in DFDL

This folder contains various files related to an effort to model ARINC429 messages in DFDL.

Two messages related to the ARINC 710-10 specification for an Instrument Landing System (ILS) are modeled:

  1. Localizer deviation (label 173 octal);
  2. Glide slope deviation (label 174 octal).

In each case, two files are supplied:

  1. a DFDL schema (extension `.dfdl.xsd`);
  2. a binary file containing the example message from the ARINC 710-10 specification (extension `.bin`).
  
The DFDL schema for both messages were tested using the [daffodil tool](https://opensource.ncsa.illinois.edu/confluence/display/DFDL/Getting+Daffodil), version 2.0.
  * Parsing (transforming the binary message to an XML infoset) works well.
  * Unparsing (transforming of an XML infoset to a binary file) works only partially, as we have not found a proper way to unparse the label (least significant byte in both messages). We currently return a value of 0x00 for the label when unparsing both messages.

## Localizer deviation message

To parse the localizer deviation message, run the following command in a command prompt where the daffodil executable is on the path:
  * `daffodil parse -s localizer-deviation.dfdl.xsd -o localizer-deviation.xml localizer-deviation.bin`
  
To unparse the resulting XML infoset back to a binary file, run the following command:
  * `daffodil unparse -s localizer-deviation.dfdl.xsd -o localizer-deviation2.bin localizer-deviation.xml`
  
## Glide slope deviation message

To parse the glide slope deviation message, run the following command:
  * `daffodil parse -s glide-slope-deviation.dfdl.xsd -o glide-slope-deviation.xml glide-slope-deviation.bin`
  
To unparse the resulting XML infoset back to a binary file, run the following command:
  * `daffodil unparse -s glide-slope-deviation.dfdl.xsd -o glide-slope-deviation2.bin glide-slope-deviation.xml`
