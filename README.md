# CSM-API-String-Arugments-Support

The purpose of this library is to enhance the API parameters for Communicable State Machine (CSM). It allows for the inclusion of various data types in plain text format. Two more templates which include "Data: Get Configuration", "Data: Set Configuration" and "Data: Get Internal Status" states, are provided in the library. These templates can serve as a starting point for building your CSM module with the ability to access data stored in the '>> internal data >>' shift register.

### Supported Data Type

 - String/Path
 - Boolean
 - Integer(I8,I16,I32,I64,U8,U16,U32,U64)
 - Float(DBL/SGL)
 - Enum
 - Array
 - Cluster

#### String/Path

It Follows CSM's rule. '->|' '->' '-@' '-&' '>>' ',' ';' should be replaced with %[Hex] String before passing. You can use **CSM AdvanceAPI\CSM Make String Arguments Safe.vi**.

#### Boolean

```
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

#### Integer

```
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

#### Float(DBL/SGL)

```
Supported Format:
  - 1.2345
  - 1.23E+2
  - 1.23E-2
  - 1.23M (1.23*10^6)
  - 1.23k (1.23*10^3)
  - 1.23m (1.23*0.001)
  - 1.23u (1.23*0.000001)
```

### Enum

**Condition1**

Enum = {AAA,BBBB,CCCC}

 - String "AAA" will be converted to Enum(AAA), IntegerValue = 0
 - String "CCC" will be converted to Enum(CCC), IntegerValue = 2

**Condition2**

Enum = {1- AAA,5 - BBBB, 9 - CCCC}

 - String "AAA" will be converted to Enum(1- AAA), IntegerValue = 0
 - String "5" will be converted to Enum(5 - BBBB), IntegerValue = 1
 - String "9 - CCCC" will be converted to Enum(9 - CCCC), IntegerValue = 2

### Array

',' is used for element seperator, ';' is usd for row seperator. '[' & ']' are used for boundary symbol. If it's not in cluster, boundary symbol is not indispensable.

**Example:**

`a,b,c,d,e` and `[a,b,c,d,e]` stands for 5 elements array

```
a b c d e
```

`a;b;c;d;e` and `[a;b;c;d;e]` stands for 5 elements array

```
a
b
c
d
e
```

`a1, b1, c1, d1, e1; a2, b2, c2, d2, e2` and `[a1, b1, c1, d1, e1; a2, b2, c2, d2, e2]` stands for 2*5 2D array

```
a1 b1 c1 d1 e1
a2 b2 c2 d2 e2
```

### Cluster


':' is used for seperating name and value, ';' is usd for seperating elements. '{' & '}' are used for boundary symbol. If it's not within other array/cluster, boundary symbol is not indispensable. Not all elements should be descripted but the changing ones.
It's helpful for CSM to reduce configuration setting API numbers. You can defined the configuration within a cluster and one single setting API for the config API.

**Example:**

Suppose a cluster as below:
```
typedef cluster{
Boolean b;
String str;
U32 integer
}
```

`b:On` and `{b:On}` stands for change the input cluster's boolean b to TURE only. Other elements keep as before.

`b:On;str:abcdef` and `{b:On;str:abcdef}` stands for change the input cluster's boolean b to TURE and String str to "abcdef".  Other elements keep as before.


## Know Issue

1.  **Cluster in Array is not fully supported. Need to imporve.**
2.  **2D array in cluster is not supported now. Need to imporve.**
