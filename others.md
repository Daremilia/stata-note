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

## 在不更改数据的情况下进行操作
```
preserve
...
restore
```
# 统计描述
```
summarize y, detail
tabstat t, statistic(mean sd)
tab x1 x2
tab1 x1 x2
```
# 循环
foreach v in x y z{
    ......
}
foreach v of varlist varlist{
    ......
}
forvalues i = 1/5{
    ......
}
# 分组与排序
```
egen cat = group(x1 x2)
sort variable
bysort var1 (var2): ......
```
# 分位数
```
egen cutpoint50 = pctile(mv), p(50)
```
# 数据reshape
```
reshape wide score, i(class) j(student) 
reshape long score, i(class) j(student) 
```
# 分组统计与分组求和
```
bysort id: egen num = sum(x == "A")
bysort id: egen sum = sum(x)
```
# 手动导入数据
```
input Id str1 cons1 str1 cons2 str1 cons3 str1 cons4
1 A A B C
2 A A A A
end
```
# local and global
```
local a = 1
display `i'
global varlist x1 x2 x3 x4
reg y $varlist
```

duplicates drop x1 x2 x3 x4, force
# 删除重复项

recode x (-3/-2 = .)(-1 = .)
# 批量赋值

est store A
esttab, compress nogap b(%6.3f) scalars(r2 N) star(* 0.1 ** 0.05 *** 0.01)  title("")
# 回归结果导出

gen varname= varname[_n+1]
# 按编号索引

encode date, gen(date_)
# 自动设置值标签
```
