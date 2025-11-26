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