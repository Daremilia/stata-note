# 杂项
```
cd "C:\file"
# 切换路径

use "data.dta", clear
webuse cancer, clear
# 从本体/网络读取数据

preserve
...
restore
# 在不更改数据的情况下进行操作

summarize y, detail
tabstat t, statistic(mean sd)
tab x1 x2
tab1 x1 x2
# 统计描述

foreach v in x y z{
    ......
}
forvalues i = 1/5{
    ......
}
# 常用循环

egen cat = group(x1 x2)
# 分组

egen cutpoint50 = pctile(mv), p(50)
gen cat = 1
replace cat = 2 if mv > cutpoint50 
# 按值分组

reshape wide score, i(class) j(student) 
reshape long score, i(class) j(student) 
# 数据reshape

bysort id: egen num = sum(x == "A")
bysort id: egen sum = sum(x)
# 分组统计与分组求和

input Id str1 cons1 str1 cons2 str1 cons3 str1 cons4
1 A A B C
2 A A A A
end
# 手动导入数据

local a = 1
display `i'
global varlist x1 x2 x3 x4
reg y $varlist
# local and global

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
# 多元插值
```
mi set mlong
mi register imputed x
# 设定插值的变量为x
set seed 29390
mi impute mvn visit4 = trt visit1, add(2) replace
# 插值
# 注意：该一系列命令前不能带preserve
```
# 置换检验
```
permute died sum=r(mean), reps(100) left nodrop nowarn: summarize died if drug
```
# 面板数据模型（固定效应）
```
xtset id time
xtreg y x control_variable, fe robust
# robust减弱假设，fe固定效应，控制了id层面的差异
reg y x i.id, vce(cluster id)
# 该命令与上述命令等价
reghdfe i.industry i.race i.year, absorb(id) vce(cluster id)
# 多维面板固定效应估计
xtreg y x control_variable i.y, fe robust
# 双向固定效应模型，同时控制了id、time层面差异
# 将y拆分为仅与个体相关、仅与时间相关、同时与个体与时间相关的三部分
xtfmb y x
# 下文提到Fama and Macbeth的pols估计方法
xtreg y x treat#post i.post, fe
# 双重差分，其中did可理解为标准did中的did，post反应政策前还是政策后
xtreg y x treat#cat#post treat#cat treat#post cat#post i.cat i.post, fe
# 三重差分，其中did可理解为标准did中的did，post反应政策前还是政策后，cat是第三维的分类变量
xtabond y x
# 动态面板 GMM
xtthres
# 门槛模型
```

**备注**
* POLS (Pooled Ols)，混合OLS，将面板数据中所有截面混合到一起作为一个整体样本。其中Fama and Macbeth的方法为，对面板中截面分别进行回归，然后取均值。


**参考资料**
[面板数据 | 连玉君](https://www.bilibili.com/video/BV1oU4y187qY)
[reghdfe | 多维面板固定效应估计](https://zhuanlan.zhihu.com/p/96691029)

# 去心
```
center x, prefix(c)
# c_x = x-mean(x)
# prefix设定生成新变量前缀
```
# 工具变量 IV
```
ivregress 2sls y x2 x3 x4 (x1 = IV), robust
estat endog
estat firststage
estat overid

ivprobit y x2 x3 x4 (x1 = IV), robust
ivtobit y x2 x3 x4 (x1 = IV), robust
```
# 结构方程 SEM
```
alpha ......
# 信度分析
spearman ......
# 效度分析
sem ......
estat gof, stats(all)
# 拟合检验
estat eqtest
estat framework, compact
```
**参考资料**
[结构方程模型（SEM）](https://www.bilibili.com/video/BV1CN411o7tk)
# 主成分分析
```
global varlist x1 x2 x3 x4
pca $varlist, vce(normal)
# 进行一次不限定主成分数量的分析
screeplot
# 绘制碎石图，决定主成分数量
pca $varlist, comp(4) vce(normal)
# 重新进行限定了数量的主成分分析
estat anti
estat kmo
estat loadings
estat residuals
estat smc
estat summarize
```
# PSM
```
global varlist x1 x2 x3 x4
psmatch2 treatment $varlist, out(y) logit ate kernel common  
pstest $varlist, both graph
# 用于平衡性检验
twoway (histogram _pscore if pa==0, bin(100) fcolor(black%60) lcolor(black%70) lwidth(none) ylabel(, glcolor(white) nogmin nogmax nogextend) legend(off) graphregion(margin(zero) fcolor(white) lcolor(white) lwidth(none) ifcolor(white) ilcolor(white) ilwidth(none)) plotregion(margin(zero) fcolor(white) lcolor(white) lwidth(none) ifcolor(white) ilcolor(white) ilwidth(none))) (histogram _pscore if pa==1, bin(100) fcolor(blake%30) lcolor(black%60) lwidth(none) ylabel(, glcolor(white) nogmin nogmax nogextend) legend(off) graphregion(margin(zero) fcolor(white) lcolor(white) lwidth(none) ifcolor(white) ilcolor(white) ilwidth(none)) plotregion(margin(zero) fcolor(white) lcolor(white) lwidth(none) ifcolor(white) ilcolor(white) ilwidth(none))),aspectratio(1) xscale(titlegap(0) outergap(2)) xtitle(倾向得分) ytitle(密度) 
# 倾向得分直方图，用于完全覆盖假设
```
[双重差分倾向得分匹配](https://www.bilibili.com/read/cv4360682)
