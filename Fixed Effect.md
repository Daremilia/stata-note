## What is fe

```markdown
reg income gender age edu hlt year
# normal regression
reg income gender age edu hlt year i.id i.year
# control fe of id and year
```

## How to

```markdown
xtset id time
xtreg y x control_variable, fe robust
# control fe of id
# option 'robust' weaken the assumption
xtreg y x control_variable i.time, fe robust
# two way fe
reghdfe y x control_variable, absorb(id industry) vce(cluster id)
# multidimensional fe
xtfmb y x
# pooled ols regression

xtivreg2 
# iv + xt

xtabond y x
# 动态面板 GMM
xtthres
# 门槛模型
```

**POLS - pooled ols**

将面板数据中所有截面混合到一起作为一个整体样本。其中Fama and Macbeth的方法为，对面板中截面分别进行回归，然后取均值。

**Reference**

[面板数据 | 连玉君](https://www.bilibili.com/video/BV1oU4y187qY)

[reghdfe | 多维面板固定效应估计](https://zhuanlan.zhihu.com/p/96691029)


 
