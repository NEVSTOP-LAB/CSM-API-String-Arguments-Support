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


## Using within CSM - Get Module Configuration.vi

### Overview





### Introduction





### Steps





## CSM API String to Typical data types.vi

### Overview





### Introduction





### Steps





## Typical data types to CSM API String.vi

### Overview





### Introduction





### Steps





## Incorrect usage collections.vi

### Overview





### Introduction





### Steps





## CSM API String to Float.vi

### Overview





### Introduction





### Steps





## CSM API String (Float with Unit) to Float.vi

### Overview





### Introduction





### Steps





## CSM API String to Complex Numeric.vi

### Overview





### Introduction





### Steps





## CSM API String to TimeStamp.vi

### Overview





### Introduction





### Steps





## CSM API String to Enum(special format).vi

### Overview





### Introduction





### Steps





## CSM API String to Array.vi

### Overview





### Introduction





### Steps





## 1D-Cluster to CSM API String.vi

### Overview





### Introduction





### Steps





## 2D-Cluster to CSM API String.vi

### Overview





### Introduction





### Steps





## Cluster to CSM API String.vi

### Overview





### Introduction





### Steps





## CSM API String to Cluster.vi

### Overview





### Introduction





### Steps





## CSM API String to Cluster with 2D Array elements.vi

### Overview





### Introduction





### Steps





# Addons - MassData Parameter Support

## MassData Argument Format.vi

### Overview





### Introduction





### Steps





## Show MassData Cache Status in FP.vi

### Overview





### Introduction





### Steps





## MassData in Non-CSM Framework.vi

### Overview





### Introduction





### Steps





## MassData in CSM.vi

### Overview





### Introduction





### Steps





# Addons - INI Static Variable Support

## Used as parameters parsed by CSM.vi

### Overview





### Introduction





### Steps





## Load the corresponding configuration by providing the prototype.vi

### Overview





### Introduction





### Steps





## In CSM API parameters.vi

### Overview





### Introduction





### Steps





## Multi-file configuration systemvi.vi

### Overview





### Introduction





### Steps





## Write and Read Configuration.vi

### Overview





### Introduction





### Steps





## import Config.ini with __include section.vi

### Overview





### Introduction





### Steps





## Read Nested Variables.vi

### Overview





### Introduction





### Steps