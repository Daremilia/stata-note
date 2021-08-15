# DID-basic
This can be applied to classical DID and mutiple time periods DID

## note
* no need to add i.post and i.policy
* all control varaibles should be affected by both time and id

## code
```
xtset id time
gen did = post * policy
xtreg depvar control_vars i.time, fe
```

## code - parallel trend test
```
gen period = year - time_point
forvalues i=1/6{
  gen pre_`i' = (period==-`i')
}
gen current = (period==0)
forvalues i=1/3{
  gen post_`i' = (period==`i')
}
drop pre_1
xtreg y1 pre_* current post_* i.year, fe robust

#delimit 
coefplot,
keep(pre_6 pre_5 pre_4 pre_3 pre_2 pre_1 current post_1 post_2 post_3) vertical
recast(connect) lcolor(black*0.45) lpattern(-)
ciopts(lcolor(black*0.8))
mlcolor(gs6) mfcolor(white) msize(*1.2) msymbol(h)
yline(0,lcolor(black*0.6) lwidth(*1.0))
xlabel(,labsize(*0.75) labcolor(black)  tposition(crossing) tlcolor(gs10))
ylabel(,nogrid tposition(crossing) tlcolor(gs10))
graphregion(color(gs16))
plotr(lcolor(edkblue) lpattern(1) lwidth(*1.5)) 
note(" " "   Notes: Vertical bands represent +(-)1.96 times the standard error of each point estimate", size(*0.8));
#delimit cr
```

## reference
[Specifying a difference in differences model with multiple time periods](https://stats.stackexchange.com/questions/76058/specifying-a-difference-in-differences-model-with-multiple-time-periods)

# DDD
Difference-in-difference-in-differences model

## code
```markdown
xtset id time
xtreg depvar control_vars treat#cat#post treat#cat treat#post cat#post i.time, fe
```

## reference
[Difference-in-difference-in-differences panel data](https://stats.stackexchange.com/questions/214562/difference-in-difference-in-differences-panel-data)

[Difference-in-Differences Estimation](https://www.nber.org/sites/default/files/2021-03/lect_10_diffindiffs_0.pdf)

