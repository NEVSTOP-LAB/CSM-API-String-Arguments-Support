# CSM-API-String-Arguments-Support

[English](./README.md) | [中文](./README(CN).md)

[![安装量](https://www.vipm.io/package/nevstop_lib_csm_api_string_arguments_support/badge.svg?metric=installs)](https://www.vipm.io/package/nevstop_lib_csm_api_string_arguments_support/)
[![星级评分](https://www.vipm.io/package/nevstop_lib_csm_api_string_arguments_support/badge.svg?metric=stars)](https://www.vipm.io/package/nevstop_lib_csm_api_string_arguments_support/)
[![许可证](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![GitHub下载量](https://img.shields.io/github/downloads/NEVSTOP-LAB/CSM-API-String-Arguments-Support/total)](https://github.com/NEVSTOP-LAB/CSM-API-String-Arguments-Support/releases)

该库用于增强通信状态机（CSM）的API参数功能，支持以纯文本格式传递各种数据类型，且特别优化了手动输入体验。

库中提供了两个新的CSM模板，它们都包含"Data: Get Configuration"和"Data: Set Configuration"两个内置状态，用于访问存储在'>> internal data >>'移位寄存器中的配置数据。

![example](.github/doc.png)

## 支持的数据类型

- 字符串 (String)
- 路径 (Path)
- 布尔值 (Boolean)
- 标签 (Tag)
- 引用号 (Refnum，包括IVI/VISA/UserDefinedRefnumTag)
- 整数 (I8,I16,I32,I64,U8,U16,U32,U64)
- 浮点数 (DBL/SGL)
- 复数 (DBL/SGL)
- 时间戳 (Timestamp)
- 枚举 (Enum)
- 数组 (Array)
- 簇 (Cluster)
- 其他类型 (使用CSM-Hexstr表示)

### 字符串(String)/路径(Path)/引用号(Refnum)/标签(Tag)

字符串和路径类型遵循CSM的规则，特殊字符如'->|'、'->'、'-@'、'-&'、'>>'、','和';'在传递前会自动转换为%[十六进制]字符串，效果等同于使用**CSM AdvanceAPI\CSM Make String Arguments Safe.vi**。

> [!NOTE]
> LabVIEW的引用号(Refnum，包括IVI/VISA/UserDefinedRefnumTag)和标签(Tag)也支持，转换规则与String类型相同。

### 布尔值(Boolean)

内置的TRUE/FALSE字符串对:

```text
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
> 空字符串将转换为原型(Prototype)的输入值。

> [!NOTE]
> 可以使用`API String - Add Boolean Strings.vi`或`API String - Remove Boolean Strings.vi`自定义布尔字符串对。

> [!NOTE]
> 标签-数据对(Tag:Value)可以被正确解析。标签仅用于提高可读性，转换过程中会被忽略。

### 整数(I8,I16,I32,I64,U8,U16,U32,U64)

支持的数据格式包括：普通整数、十进制、十六进制、二进制、八进制，以及带后缀的表示方法。例如：

```text
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
> 空字符串将转换为原型(Prototype)的输入值。

> [!NOTE]
> 标签-数据对(Tag:Value)可以被正确解析。标签仅用于提高可读性，转换过程中会被忽略。

### 浮点数(DBL/SGL)

支持的数据格式包括：普通浮点数、科学计数法以及特殊浮点数。例如：

```text
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

> [!NOTE]
> 空字符串将转换为原型(Prototype)的输入值。

> [!NOTE]
> 默认精度为6位有效数字。可以通过`API String - Set Float Precision.vi`修改精度。

> [!NOTE]
> 标签-数据对(Tag:Value)可以被正确解析。标签仅用于提高可读性，转换过程中会被忽略。

> [!NOTE]
> 带有单位的浮点数字符串也支持正确解析。

_**特殊情况说明**_:

- 如果浮点数字符串与单位字符串之间存在空格，则浮点数后面的所有内容（包括符号）都被识别为单位字符串。

> 1.23mA : 浮点数: 1.23m; 单位: A
> 1.23 mA : 浮点数: 1.23; 单位: mA

- 对于科学计数法表示的浮点数，无论是否存在空格，浮点数后面的字符串都被识别为单位字符串。

> 1.23E+5mA: 浮点数: 1.23E+5; 单位: mA
> 1.23E+5 mA: 浮点数: 1.23E+5; 单位: mA

> [!NOTE]
> `e`、`-e`、`pi`、`-pi`、`inf`、`+inf`、`-inf` 和 `NaN` 等特殊浮点数值不支持单位。

### 复数(DBL/SGL)

`a+bi` 或 `a-bi` 格式表示复数。其中 `a` 和 `b` 支持所有浮点数的表达方式。

> [!NOTE]
> 空字符串将转换为原型(Prototype)的输入值。

> [!NOTE]
> 标签-数据对(Tag:Value)可以被正确解析。标签仅用于提高可读性，转换过程中会被忽略。

### 时间戳(Timestamp)

时间戳的标准格式为：`TimeStamp_String(Format_String)`。其中 `Format_String` 用于解析 `TimeStamp_String`。

```text
"2023-10-11 22:54:33(%<%Y-%m-%d %H:%M:%S>T)" 等效于 UTC 时间戳 "2023-10-11T14:54:33.000Z".
```

_**特殊情况说明**_:

当字符串不包含 `Format_String` 时，`TimeStamp_String` 应符合 ISO8601 UTC 标准格式。

```text
"2023-10-31T14:49:39.597Z" 为有效的表达方式.
"2023-10-31T22:49:39.597+08:00" 为有效的表达方式.
```

> [!NOTE]
> 对于时间戳，空字符串将转换为当前时间。

### 枚举(Enum)

`Indexed Enum` 定义为 [索引编号(index)][分隔符(separator)][枚举字符串] 格式的字符串。支持以下表达方式：

> 十进制数字作为索引，== 作为分隔符：
>
> - 1 == boolean
> - 2 == string
> - 4 == dbl
> - 8 == number
>
> 十六进制数字作为索引，-- 作为分隔符：
>
> - 0x01 -- boolean
> - 0x02 -- string
> - 0x04 -- dbl
> - 0x08 -- number
>
> 二进制数字作为索引，__ 作为分隔符：
>
> - 0b0001 __ boolean
> - 0b0010 __ string
> - 0b0100 __ dbl
> - 0b1000 __ number

> [!NOTE]
> 索引编号(index)支持所有整数的表达方式。

_**转换规则1: 没有索引编号时**_

当没有索引编号时，通过字符串匹配进行转换。

示例：Enum = {AAA, BBBB, CCCC}

- 字符串 "AAA" 将转换为 Enum(AAA)，数字值为 0
- 字符串 "CCC" 将转换为 Enum(CCC)，数字值为 2

_**转换规则2：包含索引编号时**_

当包含索引编号时，既可以通过字符串匹配转换，也可以通过索引编号匹配转换。

示例：Enum = {1- AAA, 5 - BBBB, 9 - CCCC}

- 字符串 "AAA" 将转换为 Enum(1- AAA)，数字值为 0
- 字符串 "5" 将转换为 Enum(5 - BBBB)，数字值为 1
- 字符串 "9 - CCCC" 将转换为 Enum(9 - CCCC)，数字值为 2

### 数组(Array)

逗号(,) 用于元素分隔，分号(;) 用于行分隔。方括号([ 和 ]) 用作边界符号。对于非复杂的混合数据类型，方括号可以省略。

**示例:**

`a,b,c,d,e` 和 `[a,b,c,d,e]` 都表示一个包含5个元素的数组：

```text
a b c d e
```

`a;b;c;d;e` 和 `[a;b;c;d;e]` 都表示一个包含5个元素的数组：

```text
a
b
c
d
e
```

`a1, b1, c1, d1, e1; a2, b2, c2, d2, e2` 和 `[a1, b1, c1, d1, e1; a2, b2, c2, d2, e2]` 表示一个 2×5 的二维数组：

```text
a1 b1 c1 d1 e1
a2 b2 c2 d2 e2
```

> [!NOTE]
> 空字符串将转换为原型(Prototype)的输入值。

### 簇(Cluster)

**转换规则1: 标签-数据对(Tag:Value)模式**

在标签-数据对模式下，输入字符串由多个标签-数据对组成，冒号(:)用于分隔标签和数据，分号(;)用于分隔不同元素。花括号({ 和 })用作边界符号。对于非复杂的混合数据类型，花括号可以省略。其他规则如下：

- 标签对应簇中元素的名称，值会根据对应元素的数据类型进行转换。
- 只需描述需要修改的元素，与数据原型一致的元素可以省略。
- 通过名称匹配元素，顺序无关紧要。
- 对于嵌套簇，子簇元素的标签格式为"父簇标签.子簇元素标签"。
- 嵌套簇中，如果子簇元素的标签名称唯一，可以省略父簇的标签。

**示例:**

假设有一个簇的数据原型如下：

```text
typedef cluster{
  Boolean b;
  String str;
  U32 integer
  Cluster subCluster
  {
    Boolean b2;
  }
}
```

> `b:On` 和 `{b:On}` 都表示仅将簇中的布尔类型数据 `b` 设置为 TRUE。其他元素的值保持原型输入值不变。
>
> `b:On;str:abcdef` 和 `{b:On;str:abcdef}` 都表示将簇中的布尔类型数据 `b` 设置为 TRUE，字符串类型数据 `str` 设置为 "abcdef"。其他元素的值保持原型输入值不变。
>
> `{subCluster.b2:On}` 表示将簇中子簇的布尔类型数据 `b2` 设置为 TRUE。其他元素的值保持原型输入值不变。由于 `b2` 是唯一的，可以省略父簇标签，直接使用 `b2:On` 也表示相同的转换。

**转换规则2: 无标签模式**

对于簇，也支持仅输入数据字符串，各值之间用分号分隔。

- 顺序非常重要。第一个元素值将设置给簇的第一个元素，第二个元素值设置给簇的第二个元素，以此类推。
- 如果输入字符串的元素数量少于簇的元素数量，剩余的元素将保持不变。
- 如果输入字符串的元素数量多于簇的元素数量，多余的元素将被忽略。


> `on;abcdef;13` 和 `{on;abcdef;13}` 都表示将簇中的 `b` 更改为 TRUE，`str` 更改为 "abcdef"，U32整数更改为 13。如果簇有更多元素，它们将保持不变。
>
> `on;abcdef` 和 `{on;abcdef}` 都表示将簇中的 `b` 更改为 TRUE，`str` 更改为 "abcdef"。其他元素的值保持原型输入值不变。

> [!NOTE]
> 空字符串将转换为原型(Prototype)的输入值。

### 其他类型(使用CSM-Hexstr)

其他数据类型将首先被转换为变体(Variant)，然后使用CSM-HexStr进行表示和转换。
