---
layout: post
title: 在R下面实现对中图分类号排序的简便方法
---

这几天捣腾一些图书的数据，有一个需求，希望能对图书的索取号进行排序。First things first，先解决掉中图分类号的排序问题再说。

## 中图分类号排序如何实现

中图分类号由一位或两位英语字母，接上一串阿拉伯数字构成。之后还可能有圆括号、等号、双引号或者横杠等组成的复分代码。随意举出几个例子：`K825.5=6`、`F729(225)`、`H319.4:I712`、`B94-49`、`I561.45`。

一般的编程语言都能对字符串按照ASCII代码的顺序进行排列，而中图分类号不能直接使用编程语言内置排序功能的原因，主要是复分符号引起的。因此，要点就在于用其他的符号替换掉复分符号，然后就能利用比较字符串大小的方式来自动排序。

### 第一步：清理掉用于分割的`.`和`)`

```
library(stringr)
# Clearing all dots in CLC numbers
cla_num <- str_replace_all(cla_num, '\\.', "")

# Clearing all right parenthesis in CLC numbers
cla_num <- str_replace_all(cla_num, '\\)', "")
```
### 替换复分符号

用!、#、$分别替换掉-、(、=这三个复分符号。虽然中图分类号中还会出现双引号等复分符号，由于我这次数据在分类时只使用了-、(、=这三种复分符号，因此其他的复分符号暂时没有考虑在内。可以自己调整代码来进一步完善排序功能。

```
cla_num <- str_replace_all(cla_num, '\\-', '!')
cla_num <- str_replace_all(cla_num, '\\(', '#')
cla_num <- str_replace_all(cla_num, '\\=', '$')
```

### 将包含等号组配的分类号分割成两个字号码
```
if (str_detect(cla_num, pattern = ":")) {
  cla_num_sep <- str_split(cla_num, pattern = ":", simplify = T)
  cla_num1 <- cla_num_sep[1]
  cla_num2 <- cla_num_sep[2]
} else {
  cla_num1 <- cla_num
  cla_num2 <- '.'
}
```

### 解决T开头的分类号是两位字母的问题

工业技术大类的分类号开头有两位英语字母，比如`TP`、`TS`、`TN`等。其他的大类开头仅仅一位字母。解决方法是添加一个占位的字符，来将字母和数字分别对齐。

```
pad_non_T <- function(x) {
  # Input is a CLC number without column
  len <- nchar(x)
  return_value <- ''
  if (str_sub(x, 1, 1) == "T") {
    return_value <- x
  } else {
    part1 <- str_sub(x, 1, 1)
    part2 <- ''
    if (len > 1) {
      part2 <- str_sub(x, 2, len)
    }
    return_value <- str_c(part1, '.', part2)
  }
  return (return_value)
}

cla_num1 <- pad_non_T(cla_num1)
cla_num2 <- pad_non_T(cla_num2)
```

### 搞定！连接两个字分类号即可

先用空格填充两个分类号至长度一致，然后拼在一起即可。

```
cla_num1 <- str_pad(cla_num1, width = 20, side="right", pad = " ")
cla_num2 <- str_pad(cla_num2, width = 20, side="right", pad = " ")
comparified_cla_num <- paste(cla_num1, cla_num2, sep=':'))
```

## 试验分类

首先将上面的代码合并到一个函数`comparify_cla_num()`中，然后用一些随机的分类号来测试一下排序。

注意，要实现排序，需要对R的`sort()`或者`order()`函数指定排序方法`method="radix"`，而且不能使用`dplyr`包的`arrange()`函数来进行排序。

```
to_be_ordered <- tibble(clc_orig = c('K825.5=6','K825.5-49','F729','K825.5', 'F729(225)','F729-49','H319.4:I712','B94-49','I561.45','H319.4:D','TS976','TP319'))
to_be_ordered <- mutate(to_be_ordered, clc_comparified = clc_orig)
to_be_ordered$clc_comparified <- sapply(to_be_ordered$clc_comparified, comparify_cla_num)
order_idx <- order(to_be_ordered$clc_comparified, method="radix")
already_ordered <- to_be_ordered[order_idx,]
```

下面是完成排序的效果，第一栏是原始的分类号，第二栏是处理过后、实际排序用的的分类号。

```
# A tibble: 12 x 2
   clc_orig    clc_comparified                            
   <chr>       <chr>                                      
 1 B94-49      "B.94!49             :..                  "
 2 F729        "F.729               :..                  "
 3 F729-49     "F.729!49            :..                  "
 4 F729(225)   "F.729#225           :..                  "
 5 H319.4:D    "H.3194              :D.                  "
 6 H319.4:I712 "H.3194              :I.712               "
 7 I561.45     "I.56145             :..                  "
 8 K825.5      "K.8255              :..                  "
 9 K825.5-49   "K.8255!49           :..                  "
10 K825.5=6    "K.8255$6            :..                  "
11 TP319       "TP319               :..                  "
12 TS976       "TS976               :..                  "
```

完整的代码，请查看我GitHub页面的[Repo](https://github.com/scanthony/Ordering-Chinese-Library-Classification-Numbers)。
