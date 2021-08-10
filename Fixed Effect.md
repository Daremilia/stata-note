## what is fe

```markdown
reg income gender age edu hlt year
# normal regression
reg income gender age edu hlt year i.id i.year
# control fe of id and year
```

## how to

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
# pools regression

xtivreg2 
# iv + xt

xtabond y x
# 动态面板 GMM
xtthres
# 门槛模型
```
