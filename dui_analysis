// RD analysis

* We will exploring the picking the proper binwidth and bandwidth for an RD figure

* Figures are the most curcial part of an RD analysis, so the figures need to
* look good. This involves a lot of "trial and error" -- that is, we need to look
* at a lot of figures with different parameter values to get it right

**** Import Data ****

set more off 
clear all 

global directory="/Users/desktop/"  //mac users

cd "${directory}"

import delimited "${directory}/MLDA Crime CA.csv" 
* crime rates data

**** Making different sized bins for plotting ****

// Try plotting the data
twoway(scatter murder_r days_to_21)
// what a mess

* Instead make bins that get averages over a given range
* The size of that range is the binwidth

* Get a person's age based off of the 'days to 21' variable
gen age = 21 + days_to_21/365

* Important: cannot just make bins equally, because you do not want a bin to 
* average across the threshold (i.e. you don't want people that are almost 21 to be
* in the same bin with people that just turned 21)
* Floor-> Makes it round to the lowest integer
gen age_1day = 21 + days_to_21/365
gen age_4day = 21 + 4*floor(days_to_21/4)/365 + 2/365
gen age_20day = 21 + 20*floor(days_to_21/20)/365 + 10/365
gen age_30day = 21 + 30*floor(days_to_21/30)/365 + 15/365
gen age_40day = 21 + 40*floor(days_to_21/40)/365 + 20/365
gen age_100day = 21 + 100*floor(days_to_21/100)/365 + 50/365

**** Make a plot with different sized binwidths ****

* The preserve command makes a temporary copy of your data set that you can go back to
* Collapse is an aggregating command (in this case, it makes a mean for each group)

preserve
collapse (mean) aggravated_assault_r age, by(age_1day)
#delimit ;
graph twoway (scatter aggravated_assault_r age_1day)
	if age >= 19 & age <= 23,
	xtitle(Age)
	ytitle(Aggrevated Assaults Arrest Rate)
	note(bin size = 1 days)
	legend(off)
	scheme(s1mono)
	name(bin1, replace)
	;
#delimit cr
restore

preserve
collapse (mean) aggravated_assault_r age, by(age_4day)
#delimit ;
graph twoway (scatter aggravated_assault_r age_4day)
	if age >= 19 & age <= 23,
	xtitle(Age)
	ytitle(Aggrevated Assaults Arrest Rate)
	note(bin size = 4 days)
	legend(off)
	scheme(s1mono)
	name(bin2, replace)
	;
#delimit cr
restore

preserve
collapse (mean) aggravated_assault_r age, by(age_20day)
#delimit ;
graph twoway (scatter aggravated_assault_r age_20day if aggravated_assault_r)
	if age >= 19 & age <= 23,
	xtitle(Age)
	ytitle(Aggrevated Assaults Arrest Rate)
	note(bin size = 20 days)
	legend(off)
	scheme(s1mono)
	name(bin3, replace)
	;
#delimit cr
restore

preserve
collapse (mean) aggravated_assault_r age, by(age_30day)
#delimit ;
graph twoway (scatter aggravated_assault_r age_30day)
	if age >= 19 & age <= 23,
	xtitle(Age)
	ytitle(Aggrevated Assaults Arrest Rate)
	note(bin size = 30 days)
	legend(off)
	scheme(s1mono)
	name(bin4, replace)
	;
#delimit cr
restore

preserve
collapse (mean) aggravated_assault_r age, by(age_40day)
#delimit ;
graph twoway (scatter aggravated_assault_r age_40day)
	if age >= 19 & age <= 23,
	xtitle(Age)
	ytitle(Aggrevated Assaults Arrest Rate)
	note(bin size = 40 days)
	legend(off)
	scheme(s1mono)
	name(bin5, replace)
	;
#delimit cr
restore

preserve
collapse (mean) aggravated_assault_r age, by(age_100day)
#delimit ;
graph twoway (scatter aggravated_assault_r age_100day)
	if age >= 19 & age <= 23,
	xtitle(Age)
	ytitle(Aggrevated Assaults Arrest Rate)
	note(bin size = 100 days)
	legend(off)
	scheme(s1mono)
	name(bin6, replace)
	;
#delimit cr
restore

graph combine bin1 bin2 bin3 bin4 bin5 bin6, graphregion(style(none) color(gs16))
graph export "choosing_binwidth.pdf", replace

**** Make a plot with different sized bandwidths ****

// let's choose what the bandwidth should be. This is the range along the x-axis
// Should hold binwidth contant here

preserve
collapse (mean) aggravated_assault_r age, by(age_40day)
#delimit ;
graph twoway (scatter aggravated_assault_r age_40day)
	if age >= 17 & age <= 25,
	xtitle(Age)
	ytitle(Aggrevated Assaults Arrest Rate)
	note(bin size = 40 days)
	legend(off)
	scheme(s1mono)
	name(band1, replace)
	;
#delimit cr
restore

preserve
collapse (mean) aggravated_assault_r age, by(age_40day)
#delimit ;
graph twoway (scatter aggravated_assault_r age_40day)
	if age >= 20.9 & age <= 21.1,
	xtitle(Age)
	ytitle(Aggrevated Assaults Arrest Rate)
	note(bin size = 40 days)
	legend(off)
	scheme(s1mono)
	name(band2, replace)
	;
#delimit cr
restore

preserve
collapse (mean) aggravated_assault_r age, by(age_40day)
#delimit ;
graph twoway (scatter aggravated_assault_r age_40day)
	if age >= 20 & age <= 22,
	xtitle(Age)
	ytitle(Aggrevated Assaults Arrest Rate)
	note(bin size = 40 days)
	legend(off)
	scheme(s1mono)
	name(band3, replace)
	;
#delimit cr
restore

preserve
collapse (mean) aggravated_assault_r age, by(age_40day)
#delimit ;
graph twoway (scatter aggravated_assault_r age_40day)
	if age >= 19 & age <= 23,
	xtitle(Age)
	ytitle(Aggrevated Assaults Arrest Rate)
	note(bin size = 40 days)
	legend(off)
	scheme(s1mono)
	name(band4, replace)
	;
#delimit cr
restore
graph combine band1 band2 band3 band4, scheme(s1mono)
graph export "choosing_bandwidth.pdf", replace



**** Regressions ****

* same rd variables we made last week
gen agec = age - 21
gen agec_sq = agec^2
gen agec_cu = agec^3
gen post = 1 if agec >= 0
replace post = 0 if agec < 0
gen agec_post = agec*post
gen agec_sq_post = agec_sq*post
gen agec_cu_post = agec_cu*post

* one more, a dummy for if it was their birthday or not
* might think the effect is all from bday celebrations gone wrong
gen birthday = 1 if age == 21
replace birthday = 0 if age != 21

* run the regressions
reg aggravated_assault_r post agec agec_post if age >= 19 & age <= 23
outreg2 using "section6regs.tex", dec(3) replace
reg aggravated_assault_r post agec agec_post birthday if age >= 19 & age <= 23
outreg2 using "section6regs.tex", dec(3) append
reg aggravated_assault_r post agec agec_post agec_sq agec_sq_post birthday if age >= 19 & age <= 23
outreg2 using "section6regs.tex", dec(3) append
//etc...


**** Make a plot with the regression lines on top ****

preserve
collapse (mean)  age  aggravated_assault_r, by(age_40day)
reg aggravated_assault_r age_40day  if age >= 19 & age < 21
predict fitted_left if age >= 19 & age < 21
reg aggravated_assault_r  age_40day if age >= 21 & age < 23
predict fitted_right if age >= 21 & age < 23

#delimit ;
graph twoway (scatter aggravated_assault_r age_40day)
	(line fitted_left age_40day, lcolor(red))
	(line fitted_right age_40day, lcolor(red))
	if age >= 19 & age <= 23,
	title(Assault Arrests Age Profile)
	xtitle(Age)
	ytitle(Arrests)
	legend(off)
	scheme(s1mono)
	;
#delimit cr
graph export "section6figure.pdf", replace
restore


