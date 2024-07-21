# CSM-API-String-Arguments-Support

[![Image](https://www.vipm.io/package/nevstop_lib_csm_api_string_arguments_support/badge.svg?metric=installs)](https://www.vipm.io/package/nevstop_lib_csm_api_string_arguments_support/)
[![Image](https://www.vipm.io/package/nevstop_lib_csm_api_string_arguments_support/badge.svg?metric=stars)](https://www.vipm.io/package/nevstop_lib_csm_api_string_arguments_support/)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![GitHub all releases](https://img.shields.io/github/downloads/NEVSTOP-LAB/CSM-API-String-Arguments-Support/total)](https://github.com/NEVSTOP-LAB/CSM-API-String-Arguments-Support/releases)

The purpose of this library is to enhance the API parameters for Communicable State Machine (CSM). It allows for the inclusion of various data types in plain text format. Two more templates which include "Data: Get Configuration", "Data: Set Configuration" and "Data: Get Internal Data" states, are provided in the library. These templates can serve as a starting point for building your CSM module with the ability to access data stored in the '>> internal data >>' shift register.

![example](.github/doc.png)

## Supported Data Type

- String/Path
- Boolean
- Integer(I8,I16,I32,I64,U8,U16,U32,U64)
- Float(DBL/SGL)
- Complex(DBL/SGL)
- Timestamp
- Enum
- Array
- Cluster
- Other(use CSM-Hexstr)

### String/Path

It Follows CSM's rule. '->|' '->' '-@' '-&' '>>' ',' ';' should be replaced with %[Hex] String before passing. You can use **CSM AdvanceAPI\CSM Make String Arguments Safe.vi**.

### Boolean

_**special case**_:

- For Boolean, empty string will be converted to the input prototype value

``` text
TRUE/FALSE String Pairs:
  - T/F
  - True/False
  - On/Off
  - Enable/Disable
  - Active/Inactive
  - valid/Invalid
  - 1/0
  - Open/Close
  - Non-null/null
```

> [!NOTE]
> You  can use `API String - Add Boolean Strings.vi` or `API String - Remove Boolean Strings.vi` to set your own boolean string to be used.

> [!NOTE]
> Tag-value pair could be parsed correctly. Tag will be stripped before conversion.

### Integer

_**special case**_:

- For Integer, empty string will be converted to the input prototype value

``` text
Supported format:
  - 12345
  - 0d12345
  - 0x1234
  - 0b10101010
  - 0o777
  - 1234H
  - 10101010H
  - 7777O
  - 10k
  - 1M
```

> [!NOTE]
> Tag-value pair could be parsed correctly. Tag will be stripped before conversion.

### Float(DBL/SGL)

_**special case**_:

- For SGL/DBL, empty string will be converted to the input prototype value.

``` text
Supported Format:
  - 1.2345
  - 1.23E+2
  - 1.23E-2
  - 1.23Y (1.23*10^24)
  - 1.23Z (1.23*10^21)
  - 1.23E (1.23*10^18)
  - 1.23P (1.23*10^15)
  - 1.23T (1.23*10^12)
  - 1.23G (1.23*10^9)
  - 1.23M (1.23*10^6)
  - 1.23k (1.23*10^3)
  - 1.23m (1.23*0.001)
  - 1.23u (1.23*0.000001)
  - 1.23n (1.23*10^-9)
  - 1.23p (1.23*10^-12)
  - 1.23f (1.23*10^-15)
  - 1.23a (1.23*10^-18)
  - 1.23z (1.23*10^-21)
  - 1.23y (1.23*10^-24)
  - Special Float: `e`,`-e`,`pi`,`-pi`,`inf`,`+inf`,`-inf`,`NaN`
```

> [!NOTE]
> Default precision is 6. You can change it by `API String - Set Float Precision.vi`

> [!NOTE]
> Tag-value pair could be parsed correctly. Tag will be stripped before conversion.

> [!NOTE]
> Float String with Unit is also supported.

_**special case**_:

- If there is a `space` between float string and unit string, all strings include notation behind float string is recognized as unit string.

> 1.23mA : Float: 1.23m; Unit: A
> 1.23 mA : Float: 1.23; Unit: mA

- For scientific notation mode, any string behind the float string is recognized as unit string.

> 1.23E+5mA: Float: 1.23E+5; Unit: mA
> 1.23E+5 mA: Float: 1.23E+5; Unit: mA

- Unit is not supported for `e`,`-e`,`pi`,`-pi`,`inf`,`+inf`,`-inf`,`NaN`

### Complex(DBL/SGL)

String of `a+bi` or `a-bi` stands of complex data type. `a` and `b` is supporting all Float format.

_**special case**_:

- For Complex, empty string will be converted to the input prototype value

> [!NOTE]
> Tag-value pair could be parsed correctly. Tag will be stripped before conversion.

### Timestamp

_**special case**_:

- For Timestamp, empty string will be converted to current time.

_**Condition1**_

`TimeStamp String(FormatString)`is supported. `FormatString` in "" will be used to parse `TimeStamp String`.

``` text
"2023-10-11 22:54:33(%<%Y-%m-%d %H:%M:%S>T)" equal to UTC timestamp string "2023-10-11T14:54:33.000Z".
```

_**Condition2**_

No time string format included in string, ISO8601 UTC standard is used.

``` text
"2023-10-31T14:49:39.597Z" is valid.
"2023-10-31T22:49:39.597+08:00" is valid.
```

### Enum

Indexed Enum is defined as Enum Strings composed with [number] [separator] [enum String].

> hex Number, -- as separator
>
> - 0x01 -- boolean
> - 0x02 -- string
> - 0x04 -- dbl
> - 0x08 -- number
>
> binary Number, -- as separator
>
> - 0b0001 __ boolean
> - 0b0010 __ string
> - 0b0100 __ dbl
> - 0b1000 __ number
>
> Decided Number, == as separator
>
> - 1 == boolean
> - 2 == string
> - 4 == dbl
> - 8 == number

_**Condition1**_

Enum = {AAA,BBBB,CCCC}

- String "AAA" will be converted to Enum(AAA), IntegerValue = 0
- String "CCC" will be converted to Enum(CCC), IntegerValue = 2

_**Condition2**_

Enum = {1- AAA,5 - BBBB, 9 - CCCC}

- String "AAA" will be converted to Enum(1- AAA), IntegerValue = 0
- String "5" will be converted to Enum(5 - BBBB), IntegerValue = 1
- String "9 - CCCC" will be converted to Enum(9 - CCCC), IntegerValue = 2

### Array

',' is used for element separator, ';' is usd for row separator. '[' & ']' are used for boundary symbol. If it's not in cluster, boundary symbol is not indispensable.

_**special case**_:

- Empty String will be ignored and the prototype input will be used as output.

**Example:**

`a,b,c,d,e` and `[a,b,c,d,e]` stands for 5 elements array

``` text
a b c d e
```

`a;b;c;d;e` and `[a;b;c;d;e]` stands for 5 elements array

``` text
a
b
c
d
e
```

`a1, b1, c1, d1, e1; a2, b2, c2, d2, e2` and `[a1, b1, c1, d1, e1; a2, b2, c2, d2, e2]` stands for 2*5 2D array

``` text
a1 b1 c1 d1 e1
a2 b2 c2 d2 e2
```

### Cluster

':' is used for separating name and value, ';' is usd for separating elements. '{' & '}' are used for boundary symbol. If it's not within other array/cluster, boundary symbol is not indispensable.
It's helpful for CSM to reduce configuration setting API numbers. You can defined the configuration within a cluster and one single setting API for the config API.

_**special case**_:

- Empty String will be ignored and the prototype input will be used as output.
- if no name is given, the string input will be converted to the first element of cluster. This is useful to make the first element primary.

**Example:**

Suppose a cluster as below:

``` text
typedef cluster{
Boolean b;
String str;
U32 integer
}
```

**Case 1: Tag:Value Mode**

In this mode, the input string is a list of tag:value pairs. The tag is the name of the element in the cluster, and the value is the value to be set. The tag and value are separated by a colon. The tag:value pairs are separated by a semicolon. Not all elements should be described but the changing ones. The order of the elements is not important.

> `b:On` and `{b:On}` stands for change the input cluster's boolean b to TRUE only. Other elements keep as before.
>
> `b:On;str:abcdef` and `{b:On;str:abcdef}` stands for change the input cluster's boolean b to TRUE and String str to "abcdef".  Other elements keep as before.

**Case 2: Value Only Mode**

In this mode, the input string is a list of values. The values are separated by a semicolon. The order of the elements is important. The first element will be set to the first element of the cluster, the second element will be set to the second element of the cluster, and so on. If the input string has fewer elements than the cluster, the remaining elements will be unchanged.

> `on;abcdef;13` and `{on;abcdef;13}` stands for change the input cluster's boolean b to TRUE and String str to "abcdef", U32 integer to 13. If the cluster has more elements, they will keep as before.

**Case 3: First Element Mode**

In this mode, the input string is a single value. The first element of the cluster will be set to this value. The remaining elements will be unchanged.

> `On`,`{On}` are similar to `{b:On}`. The first element of cluster will be changed to TRUE.

#### Other DataType

Other Datatype will be treated as variant and use CSM-HexStr for data transformation.

## Know Issue

1. **Cluster in Array is not fully supported. Need to improve.**
2. **2D array in cluster is not supported now. Need to improve.**
