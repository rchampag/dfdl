# ARINC429 message label modeling in DFDL

This project contains various files related to an effort to model ARINC429 messages in DFDL. More specifically, we are trying to model the label (least significant 8 bits) of the message.

Two main files are supplied:

  1. a DFDL schema: `localizer-deviation-label.dfdl.xsd`
  2. a binary file containing the example message: `localizer-deviation-label.bin`
  
The DFDL schema was tested using the [daffodil tool](https://opensource.ncsa.illinois.edu/confluence/display/DFDL/Getting+Daffodil), version 2.0.

We are currently unable to correctly parse the message as desired, because the bitOrder attribute does not seem to be taken into account. The message is parsed, but the value obtained is incorrect.

The decimal value of the label is encoded over 8 bits, which form the least significant byte of a 4-byte ARINC429 message. Only the label (one byte) is included in this example. The bit ordering of the label is reversed: the least significant bit of the label is bit 8, and the most significant bit of the label is bit 1. 

For example, the binary label value below corresponds to decimal value `123` (octal `173`), corresponding to the ARINC429 "Localizer deviation" message. Read from left to right, this yields value `DE` in hexadecimal, which is the value contained in the binary file.

```
Bit no.   8 7 6 5 4 3 2 1
Value     1 1 0 1 1 1 1 0
```

## Parsing the message

To parse the localizer deviation message, run the following command in a command prompt where the daffodil executable is on the path:

`daffodil parse -s localizer-deviation-label.dfdl.xsd -o localizer-deviation-label.xml localizer-deviation-label.bin`

The above operation should yield the following `localizer-deviation-label.xml`:

```xml
<localizer-deviation>
  <label>123</label>
</localizer-deviation>
```

However, we currently obtain `<label>222</label>`, which corresponds to the value obtained if the least significant bit is considered bit 1 and the most significant bit considered is bit 8.

Changing the attribute `dfdl:bitOrder` in the schema from `"leastSignificantBitFirst"` to `"mostSignificantBitFirst"` seems to have no effect on the result. 

## Unparsing the message

To unparse the resulting XML infoset back to a binary file, run the following command:

`daffodil unparse -s localizer-deviation.dfdl.xsd -o localizer-deviation-label2.bin localizer-deviation-label.xml`
