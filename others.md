# 杂项

## io

```
cd "C:\file"
use "data.dta", clear
save "data.dta", replace
webuse cancer, clear

graph save "graph.gph", replace
graph export "graph.png", replace as(png)
```

## preserve

```
preserve
...
restore
```

## 统计描述

```
summarize y, detail
tabstat t, statistic(mean sd)
tab x1 x2
tab1 x1 x2
```

## 循环

```
foreach v in x y z{
    ......
}
foreach v of varlist varlist{
    ......
}
forvalues i = 1/5{
    ......
}
```

## 分组与排序

```
egen cat = group(x1 x2)
sort variable
bysort var1 (var2): ......
```

## 分位数

```
egen cutpoint50 = pctile(mv), p(50)
```

## 数据reshape

```
reshape wide score, i(class) j(student) 
reshape long score, i(class) j(student) 
```

## 分组统计与分组求和

```
bysort id: egen num = sum(x == "A")
bysort id: egen sum = sum(x)
```

## 手动导入数据

```
input Id str1 cons1 str1 cons2 str1 cons3 str1 cons4
1 A A B C
2 A A A A
end
```

## Macro - 暂元

**常见用法：**

* 定义变量列表
* counter

### 局部暂元的定义与引用
```
local number 1
di `number'
local letter a b c
di "`letter'"
```
### 两种引用方式的区别
```
local number 2+2
di `number'
> 4
di "`number'"
> 2+2
```
### 局部暂元与全局暂元
```
global num 1
local num 2
macro list
> num: 1
> _num: 2
```
### 操作暂元
```
macro list
macro drop
```

## 删除重复项

```
duplicates drop x1 x2 x3 x4, force
```

## 批量赋值

```
recode x (-3/-2 = .)(-1 = .)
```

## esttab - 回归结果导出

```
est store A
esttab, compress nogap b(%6.3f) scalars(r2 N) star(* 0.1 ** 0.05 *** 0.01)  title("")
```

## 按编号索引

```
gen varname= varname[_n+1]
```

## 值标签相关

```
encode sex, gen(gender)
# 通过字符串变量，设置新的以该变量为值标签的数值变量
decode gender, gen(sex)
# 将某个变量的值标签生成为一个新的变量
```

