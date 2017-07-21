# ARINC429 message modeling in DFDL

This folder contains various files related to an effort to model ARINC429 messages in DFDL.

Two messages related to the ARINC 710-10 specification for an Instrument Landing System (ILS) are modeled:

  1. Localizer deviation (label 173 octal, 123 decimal);
  2. Glide slope deviation (label 174 octal, 124 decimal).

In each case, two files are supplied:

  1. a DFDL schema (extension `.dfdl.xsd`);
  2. a binary file containing the example message from the ARINC 710-10 specification (extension `.bin`).
  
The DFDL schema for both messages were tested using the [daffodil tool](https://opensource.ncsa.illinois.edu/confluence/display/DFDL/Getting+Daffodil), version 2.0. Parsing (transforming the binary message to an XML infoset) and unparsing (transforming an XML infoset to a binary file) both work well, but the label poses an interesting challenge as the dfdl attribute bitOrder does not work on full bytes, and parsing/unparsing in this situation requires using dfdl:inputValueCalc for bitwise parsing, and dfdl:outputValueCalc for bitwise unparsing.

## Localizer deviation message

The following image details the structure of this message for an example deviation of +0.021 DDM (source: ARINC 710-10):

![localizer-deviation message structure](https://github.com/rchampag/dfdl/blob/master/arinc429-ils/localizer-deviation.png)

Bit 29 (part of the `SSM` field in image above) is actually the sign of the data, and is included as bit 13 of the `data` field (12 bits long in image above) in the DFDL schema.

Setting bit 11 and all padding bits to zero, this message yields the following 32 bit HEX value (least significant byte first): `DE00D8E0`. This is the value contained in the `localizer-deviation.bin` file.

To parse the localizer deviation message, run the following command in a command prompt where the daffodil executable is on the path:

`daffodil parse -s localizer-deviation.dfdl.xsd -o localizer-deviation.xml localizer-deviation.bin`

The above operation should yield the following `localizer-deviation.xml`:

```xml
<localizer_deviation>
  <label>123</label>
  <sdi>0</sdi>
  <control>0</control>
  <padding>0</padding>
  <data>0.02109375</data>
  <statusMatrix>3</statusMatrix>
  <parity>1</parity>
</localizer_deviation>
```

To unparse the resulting XML infoset back to a binary file, run the following command:

`daffodil unparse -s localizer-deviation.dfdl.xsd -o localizer-deviation2.bin localizer-deviation.xml`

The above operation should yield the value `DE00D8E0` in `localizer-deviation2.bin`.

## Glide slope deviation message

The following image details the structure of this message for an example deviation of -0.125 DDM (source: ARINC 710-10):

![glide-slope-deviation message structure](https://github.com/rchampag/dfdl/blob/master/arinc429-ils/glide-slope-deviation.png)

Bit 29 (part of the `SSM` field in image above) is actually the sign of the data, and is included as bit 13 of the `data` field (12 bits long in image above) in the DFDL schema.

Setting bit 11 and all padding bits to zero, this message yields the following 32 bit HEX value: `3E0080FD`. This is the value contained in the `glide-slope-deviation.bin` file.

To parse the glide slope deviation message, run the following command:

`daffodil parse -s glide-slope-deviation.dfdl.xsd -o glide-slope-deviation.xml glide-slope-deviation.bin`

The above operation should yield the following `glide-slope-deviation.xml`:

```xml
<glide_slope_deviation>
  <label>124</label>
  <sdi>0</sdi>
  <control>0</control>
  <padding>0</padding>
  <data>-0.125</data>
  <statusMatrix>3</statusMatrix>
  <parity>1</parity>
</glide_slope_deviation>
```

To unparse the resulting XML infoset back to a binary file, run the following command:

`daffodil unparse -s glide-slope-deviation.dfdl.xsd -o glide-slope-deviation2.bin glide-slope-deviation.xml`

The above operation should yield the value `3E0080FD` in `glide-slope-deviation2.bin`.