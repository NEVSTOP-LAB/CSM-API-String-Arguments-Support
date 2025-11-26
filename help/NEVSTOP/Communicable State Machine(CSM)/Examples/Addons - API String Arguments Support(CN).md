# CSM API String Arguments Support

## 1. Empty String to Typical data types.vi

### Overview

本文展示了CSM API参数中支持的空字符串转换为典型数据类型的示例。空字符串在大多数情况下会被转换为参考数据类型连接的数值。例外场景：

- String 数据类型：空字符串会被转换为空字符串。

### Introduction

展示不同的空字符串转换为典型数据类型的示例。特别注意 String 数据类型的例外情况。界面会显示参考值和转换后的值，供用户比较。

### Steps

- step1: 所有的普通类型，空字符串会被转换为参考数据类型连接的数值。
- step2: String 数据类型，空字符串会被转换为空字符串。
- step3: timestamp 数据类型，空字符串会转换为当前时间。

## 2. CSM API String to Typical datatypes.vi

### Overview

本文展示了CSM API参数中支持的典型数据类型转换为字符串的示例。

### Introduction

本文展示了一些典型的字符串转换为典型的数据类型的实例。

### Steps

- step1: 路径字符串转换为 path 数据类型
- step2: 字符串数据类型的转换
- step3: 典型的boolean数据类型的描述可以转换为boolean数据类型。
- step4: i32 数据类型转换
- step5: dbl 数据类型的转换
- step6： 普通的enum 类型转换
- step7: 具有编号的 enum 数据类型可以只描述枚举字符串，
- step8: 具有编号的 enum 数据类型转换也可以描述索引编号
- step9: 一维数组类型转换
- step10：cluster数据类型转换
- step11: cluster array 数据类型转换
- step12: 二维数组类型转换


## 3. Incorrect usage collections.vi

### overview

一些不正确的API String 描述情况。

### Instruction

选择 Action 后，运行 VI 并查看结果，不正确的格式转换后与预期数据不匹配。

### Introduction

Cluster 数据类型是重点描述的情况，因为它的情况比较多。通常它的描述格式是：

1. 标签-数据对(Tag:Value)模式
2. 无标签模式

本范例演示了一些不正确的API String 描述情况。

## 常见普通类型数据的转换

### Float 类型(4.1 CSM API String to Float.vi)

#### Overview

API String 支持的Float 格式包括：普通浮点数、科学计数法以及特殊浮点数。本范例演示了这些格式的转换。

#### Introduction

API String 支持的Float 格式包括：普通浮点数、科学计数法以及特殊浮点数。本范例演示了这些格式的转换。例如以下方式：

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
  - 特殊浮点数值: `e`,`-e`,`pi`,`-pi`,`inf`,`+inf`,`-inf`,`NaN`
```

注意：
 - 空字符串将转换为原型(Prototype)的输入值。
 - 默认精度为6位有效数字。可以通过API String - Set Float Precision.vi修改精度。
 - 标签-数据对(Tag:Value)可以被正确解析。标签仅用于提高可读性，转换过程中会被忽略。

#### Steps

- step1: 往INF方向的浮点数转换测试
- step2: 往-INF方向的浮点数转换测试
- step3: 10...0 字符串的转换测试

### Float类型，API String 中加入单位（4.2. CSM API String (Float with Unit) to Float.vi）

#### Overview

演示API String中包含单位的浮点数转换为浮点数的示例。

#### Introduction

API String 带有单位的浮点数字符串也支持正确解析。

如果浮点数字符串与单位字符串之间存在空格，则浮点数后面的所有内容（包括符号）都被识别为单位字符串。
例如：1.23mA : 浮点数: 1.23m; 单位: A 1.23 mA : 浮点数: 1.23; 单位: mA

对于科学计数法表示的浮点数，无论是否存在空格，浮点数后面的字符串都被识别为单位字符串。
例如：1.23E+5mA: 浮点数: 1.23E+5; 单位: mA 1.23E+5 mA: 浮点数: 1.23E+5; 单位: mA

e、-e、pi、-pi、inf、+inf、-inf 和 NaN 等特殊浮点数值不支持单位。

#### Steps

- step1: 不同情况的浮点数单位转换测试
- step2：API String中的转换依赖于 String To Float_csm.vi，可以在函数选板找到这个函数。

### 复数(4.3 CSM API String to Complex Numeric.vi)

#### Overview

本范例用于演示 API String 对于复数的支持。

#### Introduction

API String 支持复数类型。a+bi 或 a-bi 格式表示复数。其中 a 和 b 支持所有浮点数的表达方式。

特殊情况说明：
- 空字符串将转换为原型(Prototype)的输入值。
- 标签-数据对(Tag:Value)可以被正确解析。标签仅用于提高可读性，转换过程中会被忽略。

### Timestamp 类型(4.4 CSM API String to Timestamp.vi)

#### Overview

本范例用于演示 API String 对于时间戳的支持。

#### Introduction

API String 时间戳的标准格式为：TimeStamp_String(Format_String)。其中 Format_String 用于解析 TimeStamp_String。

特殊情况说明:
- 当字符串不包含 Format_String 时，TimeStamp_String 应符合 RFC3339 标准格式。
- "2023-10-31T14:49:39.597Z" 为有效的表达方式.
- "2023-10-31T22:49:39.597+08:00" 为有效的表达方式.
- 对于时间戳，空字符串将转换为当前时间。

#### steps

- step1: 空字符串转换为当前时间
- step2: 标准格式转换为时间戳
- step3: timestamp转换为 API String，并被正确解析回时间戳数据类型
- step4: TimeStamp_String(Format_String)格式的示例

### Enum 类型(4.5 CSM API String to Enum(special format).vi)

#### Overview

本范例用于演示 API String 对于枚举类型的支持。

#### Introduction

API String Enum 定义为 [索引编号(index)][分隔符(separator)][枚举字符串] 格式的字符串。

- 索引编号支持NUMERIC类型的所有表达方式。例如：0x01,0b0001。
- 分隔符(separator)支持 =,-,_ 三种字符，重复个数不限。

转换规则：

- 转换规则1: 当没有索引编号时，通过字符串匹配进行转换。
- 转换规则2: 当包含索引编号时，既可以通过字符串匹配转换，也可以通过索引编号匹配转换。

示例1： 无索引编号的转换规则
```
示例：Enum = {AAA, BBBB, CCCC}

字符串 "AAA" 将转换为 Enum(AAA)，数字值为 0
字符串 "CCC" 将转换为 Enum(CCC)，数字值为 2
```

示例2： 有索引编号的转换规则
```
示例：Enum = {1- AAA, 5 - BBBB, 9 - CCCC}

字符串 "AAA" 将转换为 Enum(1- AAA)，数字值为 0
字符串 "5" 将转换为 Enum(5 - BBBB)，数字值为 1
字符串 "9 - CCCC" 将转换为 Enum(9 - CCCC)，数字值为 2
```

## Array 数据类型的支持

### 5.1 CSM API String to Array.vi

#### Overview

本范例用于演示 API String 对于数组类型的支持。

#### Introduction

API String中对于Array 的定义，逗号(,) 用于元素分隔，分号(;) 用于行分隔。方括号([ 和 ]) 用作边界符号。对于非复杂的混合数据类型，方括号可以省略。

示例：

- `a,b,c,d,e`  `[a,b,c,d,e]`，`[a;b;c;d;e]` 都表示一个包含5个元素的数组：
- `a1, b1, c1, d1, e1; a2, b2, c2, d2, e2` 和 `[a1, b1, c1, d1, e1; a2, b2, c2, d2, e2]` 表示一个 2×5 的二维数组

特殊情况说明:

- 空字符串将转换为原型(Prototype)的输入值。

#### Steps

- step1: 空字符串转换为原型(Prototype)的输入值
- step2: 一维数组转换
- step3: 二维数组转换

### 5.2 Cluster 1D Array to CSM API String.vi

#### Overview

本范例用于演示 Cluster 1D Array 的 CSM API String表达。

#### Introduction

Array 是一种复合数据类型，可能包含不同的数据类型。其中以 Cluster 最为复杂，本范例将展示1D Cluster Array 的 CSM API String 表达字符串。

### 5.3 Cluster 2D Array to CSM API String.vi

#### Overview

本范例用于演示 Cluster 2D Array 的 CSM API String表达。

#### Introduction

Array 是一种复合数据类型，可能包含不同的数据类型。其中以 Cluster 最为复杂，本范例将展示2D Cluster Array 的 CSM API String 表达字符串。





