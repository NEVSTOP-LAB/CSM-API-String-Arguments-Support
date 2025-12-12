# CSM API String Arguments Support

## 1. Empty String to Typical data types.vi

### Overview
Demonstrates how the CSM API supports converting empty strings to typical data types. In most cases, an empty string is converted to the numerical value connected to the reference data type. The following is an exception:

- **String data type:** An empty string converts to an empty string.

### Introduction
Demonstrates converting various empty strings to typical data types. Pay special attention to the exception regarding the String data type. The front panel displays the reference values and the converted values for comparison.
### Steps

- step1: For all common types, an empty string converts to the numerical value connected to the reference data type.
- step2: For the String data type, an empty string converts to an empty string.
- step3: For the Timestamp data type, an empty string converts to the current time.



## 2. CSM API String to Typical datatypes.vi

### Overview

Demonstrates the conversion of strings to typical data types supported in CSM API.

### Introduction

This example illustrates instances of converting typical strings into typical data types.

### Steps

- step1: Path string conversion to Path data type.
- step2: Conversion of String data type.
- step3: Conversion of string describing Boolean value to Boolean data type.
- step4: I32 data type conversion.
- step5: DBL (double-precision floating) data type conversion.
- step6: Common Enum type conversion.
- step7: Numbered Enum data types can describe only the enumeration string.
- step8: Numbered Enum data type conversion can also describe the index number.
- step9: 1D Array type conversion.
- step10: Cluster data type conversion.
- step11: Cluster array data type conversion.
- step12: 2D Array type conversion.



## 3. Incorrect usage collections.vi

### Overview

Demonstrates incorrect API String descriptions.

### Instruction

Select **Action**, run the VI, and view the results. Incorrect formats will result in converted data that does not match the expected data.

### Introduction

This example focuses especially on the Cluster data type, due to its complexity. Generally, the description format for Cluster data types falls into two categories:

1. Tag:Value Mode
2. No-Tag Mode

This example demonstrates some instances of incorrect API String descriptions.



## Conversion of Common Primitive Types

### Float Type (4.1 CSM API String to Float.vi)

#### Overview

Demonstrates conversion of the following Float formats that API String supports:

- Standard floating-point numbers
- Scientific notation
- Special floating-point numbers



#### Introduction

The API String supports float formats such as standard floating-point numbers, scientific notation, and special floating-point numbers. This example demonstrates conversions using formats like the following:

```
  - 1.2345
  - 1.23E+2
  - 1.23E-2
  - 1.23Y (1.23×10^24)
  - 1.23Z (1.23×10^21)
  - 1.23E (1.23×10^18)
  - 1.23P (1.23×10^15)
  - 1.23T (1.23×10^12)
  - 1.23G (1.23×10^9)
  - 1.23M (1.23×10^6)
  - 1.23k (1.23×10^3)
  - 1.23m (1.23×0.001)
  - 1.23u (1.23×0.000001)
  - 1.23n (1.23×10^-9)
  - 1.23p (1.23×10^-12)
  - 1.23f (1.23×10^-15)
  - 1.23a (1.23×10^-18)
  - 1.23z (1.23×10^-21)
  - 1.23y (1.23×10^-24)
  - Special floating-point values: `e`, `-e`, `pi`, `-pi`, `inf`, `+inf`, `-inf`, `NaN`
```

**Note:**

- An empty string converts to the prototype input value.
- The default precision is 6 significant digits. You can modify precision by using  `API String - Set Float Precision.vi`.
- Tag:Value can be parsed correctly. Tags are used only for readability and are ignored during the conversion process.

#### Steps

- step1: Conversion toward INF float values.
- step2: Conversion toward -INF float values.
- step3: Conversion with strings formatted as 10...0.



### Float with Unit (4.2. Float with Unit CSM API String to Float.vi)

#### Overview

Demonstrates the conversion of floating-point numbers with units into floating-point numbers.

#### Introduction

CSM supports parsing floating-point numbers with units.

If there is a space between the floating-point string and the unit string, everything following the floating-point number (including symbols) is recognized as the unit string. For example:

- `1.23mA` : Float: 1.23m; Unit: A
- `1.23 mA` : Float: 1.23; Unit: mA

For floating-point numbers expressed in scientific notation, the string following the number is recognized as the unit string, regardless of whether a space exists. For example:

- `1.23E+5mA`: Float: 1.23E+5; Unit: mA
- `1.23E+5 mA`: Float: 1.23E+5; Unit: mA

Special floating-point values such as `e`, `-e`, `pi`, `-pi`, `inf`, `+inf`, `-inf`, and `NaN` do not support units.

#### Steps

- step1: Testing floating-point unit conversion in various scenarios.
- step2: The conversion in the API String depends on `String To Float_csm.vi`, available on the Functions palette.



### Complex Numeric (4.3 CSM API String to Complex Numeric.vi)

#### Overview

Demonstrates API String support for complex numbers.

#### Introduction

API String supports complex numeric types. The format `a+bi` or `a-bi` represents a complex number, where `a` and `b` support all floating-point number expressions. The following are special cases:

- An empty string converts to the prototype input value.
- Tag:Value pairs can be parsed correctly. Tags are used only for readability and are ignored during the conversion process.



### Timestamp Type (4.4 CSM API String to TimeStamp.vi)

#### Overview

Demonstrates API String support for timestamps.

#### Introduction

The standard format for an API String timestamp is `TimeStamp_String(Format_String)`. The `Format_String` is used to parse the `TimeStamp_String`.

**Special Case Notes:**

- When a string does not contain `Format_String`,  `TimeStamp_String` must follow the RFC3339 standard format.
- "`2023-10-31T14:49:39.597Z`" is a valid expression.
- "`2023-10-31T22:49:39.597+08:00`" is a valid expression.
- An empty string converts to the current time.

#### Steps

- step1: Empty string converts to current time.
- step2: Standard format converts to timestamp.
- step3: Timestamp converts to API String and is correctly parsed back to Timestamp data type.
- step4: Example of the `TimeStamp_String(Format_String)` format.



### Enum Type (4.5 CSM API String to Enum(special format).vi)

#### Overview

Demonstrates API String support for enumeration (Enum) data types.

#### Introduction

The API String Enum is defined as a string in the format `[Index][Enum String]`.

- The index supports all expressions of the numeric type, such as `0x01`, `0b0001`.
- Supported separators include `=`, `-`, and `_`. There is no limit on the number of repeated separators.

**Conversion Rules:**

- **Rule 1:** When there is no index number, conversion is performed via string matching.
- **Rule 2:** When an index number is included, conversion can be performed via either string matching or index number matching.

**Example 1: Conversion rule without index number**

```
Example: Enum = {AAA, BBBB, CCCC}

String "AAA" converts to Enum(AAA), numeric value is 0
String "CCC" converts to Enum(CCC), numeric value is 2
```

**Example 2: Conversion rule with index number**

```
Example: Enum = {1- AAA, 5 - BBBB, 9 - CCCC}

String "AAA" converts to Enum(1- AAA), numeric value is 0
String "5" converts to Enum(5 - BBBB), numeric value is 1
String "9 - CCCC" converts to Enum(9 - CCCC), numeric value is 2
```



## Array Data Type Support

### 5.1 CSM API String to Array.vi

#### Overview

Demonstrates API String support for array types.

#### Introduction

In the API String definition for Arrays, comma (`,`) is used for element separation, semicolon (`;`) is used for row separation, and brackets (`[` and `]`) are used as boundary symbols. For non-complex mixed data types, brackets can be omitted.

**Examples:**

- `a,b,c,d,e`, `[a,b,c,d,e]`, and `[a;b;c;d;e]` all represent an array containing 5 elements.
- `a1, b1, c1, d1, e1; a2, b2, c2, d2, e2` and `[a1, b1, c1, d1, e1; a2, b2, c2, d2, e2]` represent a 2x5 2D array.

**Special Case Note:**

- An empty string converts to prototype input value.

#### Steps

- step1: Empty string converts to prototype input value.
- step2: 1D array conversion.
- step3: 2D array conversion.



### 5.2 Cluster 1D Array to CSM API String.vi

#### Overview

Demonstrates API String expression for 1-D array of clusters.

#### Introduction

An array is a composite data type that may contain different data types, with cluster being the most complex. This example shows the API String expression for 1-D array of clusters.



### 5.3 Cluster 2D Array to CSM API String.vi

#### Overview

Demonstrates API String expression for 2-D array of clusters.

#### Introduction

An array is a composite data type that may contain different data types, with cluster being the most complex. This example shows the API String expression for 2-D array of clusters.



## Cluster Data Type Support

### 6.1 Cluster to CSM API String.vi

#### Overview

Demonstrates API String expression for a cluster.

#### Introduction

A cluster is a complex type composed of other data types. This example shows API String expression for clusters. In an API String, a cluster can be expressed in the following two ways:

1. **Tag:Value Mode** In Tag:Value mode, the input string consists of multiple tag-value pairs. A colon (`:`) separates the tag and data, and a semicolon (`;`) separates different elements. Curly braces (`{` and `}`) are used as boundary symbols. For non-complex mixed data types, curly braces can be omitted.
2. **No-Tag Mode** Clusters also support inputting only data strings, with values separated by semicolons.

#### Steps

- step1: Tag:Value Mode.
- step2: No-Tag Mode.



### 6.2 CSM API String to Cluster.vi

#### Overview

Demonstrates the API String expression for a cluster.

#### Introduction

A cluster is a complex type composed of other data types. This example shows API String expression for clusters. In an API String, a cluster can be expressed in the following two ways:

1. **Tag:Value Mode:** In Tag:Value mode, the input string consists of multiple tag-value pairs. A colon (`:`) separates the tag and data, and a semicolon (`;`) separates different elements. Curly braces (`{` and `}`) are used as boundary symbols. For non-complex mixed data types, curly braces can be omitted. Other rules are as follows:
   - Tags correspond to the names of the elements in the cluster, and values are converted according to the data type of the corresponding element.
   - Only elements that need modification require description; elements consistent with the data prototype can be omitted.
   - Elements are matched by name; order does not matter.
   - For nested clusters, the tag format for sub-cluster elements is "ParentClusterTag.SubClusterElementTag".
   - In nested clusters, if the sub-cluster element's tag name is unique, the parent cluster's tag can be omitted.
2. **No-Tag Mode:** Clusters also support inputting only data strings, with values separated by semicolons. Order is critical. The first element value sets the first element of the cluster, the second value sets the second element, and so on.
   - If the number of elements in the input string is less than the number of elements in the cluster, the remaining elements remain unchanged.
   - If the number of elements in the input string is greater than the number of elements in the cluster, the excess elements are ignored.

#### Steps

- step1: Empty string converts to reference data.
- step2: Modifying specific element data in the reference data using only the tag name.
- step3: With no tag and only one item count, the first element data of the reference data is modified.
- step4: With no tag and only one item count, the first element data of the reference data is modified, where the first element is an array.
- step5: No-Tag Mode.
- step6: No-Tag Mode, but the element count is less than the total count.
- step7: Modifying specific element data in the reference data using only the tag name.
- step8: No-Tag Mode, but the Boolean data within uses alternative expressions.
- step9: No-Tag Mode, but the element count is greater than the total count.
- step10: Under multi-level nesting, tag names can contain a dot (`.`) to indicate the hierarchy of the nested cluster. Where there is no ambiguity, one can write only the final element name or partial sub-nested level names.



## Complex Scenarios

### 7.1 Complex Cluster in Array.vi

#### Overview

Array/Cluster nesting is a complex scenario. This example shows API String expression for a complex cluster array. However, this is not a typically recommended usage scenario.



### 7.2 Cluster with 2D Array elements.vi

#### Overview

Array/Cluster nesting is a complex scenario. This example shows API String expression for a 2-D cluster array. However, this is not a typically recommended usage scenario.