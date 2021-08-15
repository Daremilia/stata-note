# DID-basic
This can be applied to classical DID and mutiple time periods DID

## note
* no need to add i.post and i.policy
* all control varaibles should be affected by both time and id

## code
```markdown
xtset id time
gen did = post * policy
xtreg depvar control_vars i.time, fe
```

## code - parallel trend test
```markdown
gen period = year - time_point
forvalues i=1/6{
  gen pre_`i'=(period==-`i')
}
gen current= (period==0)
forvalues i=1/3{
  gen post_`i'=(period==`i')
}
drop pre_1
xtreg y1 pre_* current post_* i.year, fe robust
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

