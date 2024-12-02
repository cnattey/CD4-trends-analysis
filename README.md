****************Table 1****************************
 use "D:CD4_ANALYSIS\New_approach_clean firstcd4\first_cd4_tier_vars_All.dta"

sumntab, by(CD4_year_cat) vars (total_care gender Age Facility_type CD4_cat ) type (1 1 2 2 2) mean median total replace word
wordname(summary_table) title(Characteristics of the individual with first CD4 count in TIER.Net and NHLS 2004-2018 )



use "D:CD4_ANALYSIS\New_approach_clean firstcd4\first_cd4_nhls_vars_All.dta"

sumntab, by(CD4_year_cat) vars (gender Age Facility_type CD4_cat total_care) type (1 1 2 2 2) mean median total replace word
wordname(summary_table) title(Characteristics of the individual with first CD4 count in TIER.Net and NHLS 2004-2018 )

************* Figure 1 and supplementary 1***************
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Tier_four_vars_provinces.dta"
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
collapse (sum) first_CD4_tier,by (yearmonth)
br
gen daily_average= first_CD4_tier/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
br
line daily_average yearquarter
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Tier_daily_quarter.dta"

*****************NHLS DATA*********
gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)



gen daily_average= first_CD4_nhls/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
br
line daily_average yearquarter
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Tier_daily_quarter.dta"

use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quarter.dta"
ren daily_average daily_average_nhls
save,replace
joinby yearquarter using Tier_daily_quarter.dta

line daily_average_nhls daily_average_tier yearquarter

save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Collapsed_Tier_Nhls_All.dta"

************plot graph********

twoway (line first_CD4_nhls quarteryear, lcolor(blue)) (line first_CD4_tier quarteryear,lcolor(red)), ///
xlabel(180 "2005" 192 "2008" 204 "2011" 216 "2014"  228 "2017", labsize(medium))xmtick(#15) ///
ylabel(0(200)1200,labsize(medium)) graphregion(color(white)) bgcolor(white) ytitle("Average daily number of patients (First CD4 count)", size(medium)) ///
xtitle(Year, size(medium)) xline(226.75 230.75, lw(3) lc(gs15)) ///
xline(227 231, lc(black) lw(vthin) lp(dash)) ///
legend(order(1 "NHLS" 2 "TIER") position(4) ring(10)) legend(size(small)) ///
title(Four Provinces(excl KwaZulu-Natal), size(medium))


****************Gauteng plot********
cd "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\"
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_tier_vars_30_gp.dta"
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
collapse (sum) first_CD4_tier,by (yearmonth)
br
gen daily_average= first_CD4_tier/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
br
line daily_average yearquarter
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Tier_daily_quartergp.dta"

*****************NHLS DATA*********
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_nhls_vars_gp.dta"
gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)



gen daily_average= first_CD4_nhls/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
br
line daily_average yearquarter
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quartergp.dta"

use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quartergp.dta"
ren daily_average daily_average_nhls
save,replace
ren daily_average daily_average_tier
joinby yearquarter using Tier_daily_quartergp.dta

line daily_average_nhls daily_average_tier yearquarter
ren daily_average_nhls first_CD4_nhls
ren daily_average_tier first_CD4_tier
ren 
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Collapsed_Tier_Nhls_gp.dta"
***plot grpah********

twoway (line first_CD4_nhls yearquarter) (line first_CD4_tier yearquarter), legend(off) ///
xlabel(180 "2005" 192 "2008" 204 "2011" 216 "2014"  228 "2017", labsize(small))xmtick(#15) ///
ylabel(0(100)500,labsize(small)) graphregion(color(white)) bgcolor(white) ytitle("Average daily number of patients (First CD4 count)", size(small)) ///
xtitle(Year, size(small)) xline(226.75 230.75, lw(3) lc(gs15)) ///
xline(227 231, lc(black) lw(vthin) lp(dash)) ///
title(Gauteng (excl City of Johannesburg), size(small))


****************KZN plot********
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_tier_vars__30_kzn.dta",clear
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
collapse (sum) first_CD4_tier,by (yearmonth)
br
gen daily_average= first_CD4_tier/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
br
line daily_average yearquarter
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Tier_daily_quarterkzn.dta"
ren daily_average daily_average_tier
save,replace
*****************NHLS DATA*********
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_nhls_vars_kzn.dta",clear
gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)



gen daily_average= first_CD4_nhls/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
ren daily_average daily_average_nhls
line daily_average yearquarter
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quarterkzn.dta"

use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quarterkzn.dta"

joinby yearquarter using Tier_daily_quarterkzn.dta

line daily_average_nhls daily_average_tier yearquarter
ren daily_average_nhls first_CD4_nhls
ren daily_average_tier first_CD4_tier

gen yq= yearquarter
replace daily_average_nhls=. if yq<204
drop yq
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Collapsed_Tier_Nhls_kzn.dta",replace
***plot grpah********

twoway (line first_CD4_nhls yearquarter) (line first_CD4_tier yearquarter), legend(off) ///
xlabel(180 "2005" 192 "2008" 204 "2011" 216 "2014"  228 "2017", labsize(small))xmtick(#15) ///
ylabel(0(200) 1000,labsize(small)) graphregion(color(white)) bgcolor(white) ytitle("Average daily number of patients (First CD4 count)", size(small)) ///
xtitle(Year, size(small)) xline(226.75 230.75, lw(3) lc(gs15)) ///
xline(227 231, lc(black) lw(vthin) lp(dash)) ///
title(KwaZulu-Natal, size(small))



****************Mpumalanga plot********
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_tier_vars_30_mp.dta",clear
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
collapse (sum) first_CD4_tier,by (yearmonth)
br
gen daily_average= first_CD4_tier/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
br
line daily_average yearquarter
ren daily_average daily_average_tier
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Tier_daily_quartermp.dta",replace


*****************NHLS DATA*********
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_nhls_vars_mp.dta",clear
gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)



gen daily_average= first_CD4_nhls/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
ren daily_average daily_average_nhls
line daily_average yearquarter
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quartermp.dta",replace

use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quartermp.dta"

joinby yearquarter using Tier_daily_quartermp.dta

line daily_average_nhls daily_average_tier yearquarter
ren daily_average_nhls first_CD4_nhls
ren daily_average_tier first_CD4_tier

gen yq= yearquarter
replace daily_average_nhls=. if yq<204
drop yq
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Collapsed_Tier_Nhls_mp.dta",replace
***plot grpah********

twoway (line first_CD4_nhls yearquarter) (line first_CD4_tier yearquarter), legend(off) ///
xlabel(180 "2005" 192 "2008" 204 "2011" 216 "2014"  228 "2017", labsize(small))xmtick(#15) ///
ylabel(0(100)400,labsize(small)) graphregion(color(white)) bgcolor(white) ytitle("Average daily number of patients (First CD4 count)", size(small)) ///
xtitle(Year, size(small)) xline(226.75 230.75, lw(3) lc(gs15)) ///
xline(227 231, lc(black) lw(vthin) lp(dash)) ///
title(Mpumalanga, size(small))



****************Limpopo plot********
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_tier_vars_30_lp.dta",clear
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
collapse (sum) first_CD4_tier,by (yearmonth)
br
gen daily_average= first_CD4_tier/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
br
line daily_average yearquarter
ren daily_average daily_average_tier
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Tier_daily_quarterlp.dta",replace


*****************NHLS DATA*********
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_nhls_vars_lp.dta",clear
gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)



gen daily_average= first_CD4_nhls/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
ren daily_average daily_average_nhls
line daily_average yearquarter
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quarterlp.dta",replace

use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quarterlp.dta"

joinby yearquarter using Tier_daily_quarterlp.dta

line daily_average_nhls daily_average_tier yearquarter
ren daily_average_nhls first_CD4_nhls
ren daily_average_tier first_CD4_tier

gen yq= yearquarter
replace daily_average_nhls=. if yq<204
drop yq
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Collapsed_Tier_Nhls_lp.dta",replace
***plot grpah********

twoway (line first_CD4_nhls yearquarter) (line first_CD4_tier yearquarter), legend(off) ///
xlabel(180 "2005" 192 "2008" 204 "2011" 216 "2014"  228 "2017", labsize(small))xmtick(#15) ///
ylabel(0(100)300,labsize(small)) graphregion(color(white)) bgcolor(white) ytitle("Average daily number of patients (First CD4 count)", size(small)) ///
xtitle(Year, size(small)) xline(226.75 230.75, lw(3) lc(gs15)) ///
xline(227 231, lc(black) lw(vthin) lp(dash)) ///
title(Limpopo, size(small))

***********Nothern Cape*******************
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_tier_vars_30_nc.dta",clear
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
collapse (sum) first_CD4_tier,by (yearmonth)
br
gen daily_average= first_CD4_tier/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
br
line daily_average yearquarter
ren daily_average daily_average_tier
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Tier_daily_quarternc.dta",replace


*****************NHLS DATA*********
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_nhls_vars_nc.dta",clear
gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)



gen daily_average= first_CD4_nhls/30.5
br
line daily_average yearmonth

gen yearquarter=qofd(dofm(yearmonth))
format yearquarter %tq
br
line daily_average yearquarter
br
collapse (mean) daily_average,by(yearquarter)
ren daily_average daily_average_nhls
line daily_average yearquarter
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quarternc.dta",replace

use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quarternc.dta"

joinby yearquarter using Tier_daily_quarternc.dta

line daily_average_nhls daily_average_tier yearquarter
ren daily_average_nhls first_CD4_nhls
ren daily_average_tier first_CD4_tier

gen yq= yearquarter
replace daily_average_nhls=. if yq<204
drop yq
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\Collapsed_Tier_Nhls_nc.dta",replace
***plot grpah********

twoway (line first_CD4_nhls yearquarter) (line first_CD4_tier yearquarter), legend(off) ///
xlabel(180 "2005" 192 "2008" 204 "2011" 216 "2014"  228 "2017", labsize(small))xmtick(#15) ///
ylabel(0(10) 40,labsize(small)) graphregion(color(white)) bgcolor(white) ytitle("Average daily number of patients (First CD4 count)", size(small)) ///
xtitle(Year, size(small)) xline(226.75 230.75, lw(3) lc(gs15)) ///
xline(227 231, lc(black) lw(vthin) lp(dash)) ///
title(Nothern Cape, size(small))


graph combine All_proinces_exclKZN.gph gp.gph kzn.gph mp.gph lp.gph nc.gph




*******Pre _post daily mean plots******
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\first_cd4_tier_vars_30_gp.dta"
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
drop if yearmonth<tm(2015m3)
br
sort yearmonth
br yearmonth
collapse (sum) first_CD4_tier,by (yearmonth)
br
gen daily_average= first_CD4_tier/30.5
br
line daily_average yearmonth
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\GP_tier_daily.dta"

***NHLS GP****
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_nhls_vars_gp.dta"

gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
drop if yearmonth<tm(2015m3)

sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)
gen daily_average= first_CD4_nhls/30.5

ren daily_average daily_average_nhls
save,replace
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\GP_nhls_daily.dta"

joinby yearmonth using "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\GP_tier_daily.dta"

save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\GP_nhls_tier_daily.dta"

line daily_average_nhls daily_average_tier yearmonth
*****Graph**********
twoway (line daily_average_nhls  yearmonth) (line daily_average_tier yearmonth), ///
xlabel(662 "March 2015"  674 "March 2016"  686 "March 2017" 698 "March 2018", labsize(medsmall))xmtick(#36) ///
ylabel(0(250) 500,labsize(medlarge)) graphregion(color(white)) bgcolor(white) ytitle("Number of patients", size(medlarge)) ///
xtitle(Date of first CD4 count, size(medlarge)) xline(680, lw(3) lc(gs15)) ///
legend(order(1 "NHLS" 2 "TIER") pos(4) ring(0) region(lc(white))) legend(size(small)) ///
title(Gauteng (excl City of Johannesburg), size(medium))
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\plot_GP.dta"

**********kZN*************

use "D:CD4_ANALYSIS\New_approach_clean firstcd4\first_cd4_tier_vars__30_kzn.dta"
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
drop if yearmonth<tm(2015m3)
br
sort yearmonth
br yearmonth
gen first_CD4_tier=1
collapse (sum) first_CD4_tier,by (yearmonth)

gen daily_average= first_CD4_tier/30.5
br
line daily_average yearmonth
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\KZN_tier_daily.dta"
***NHLS KZN****
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_nhls_vars_kzn.dta"

gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
drop if yearmonth<tm(2015m3)

sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)
gen daily_average= first_CD4_nhls/30.5

ren daily_average daily_average_nhls

save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\KZN_nhls_daily.dta"

joinby yearmonth using  "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\KZN_tier_daily.dta"


save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\KZN_nhls_tier_daily.dta"

line daily_average_nhls daily_average_tier yearmonth
*****Graph**********
twoway (line daily_average_nhls  yearmonth) (line daily_average_tier yearmonth), ///
xlabel(662 "March 2015"  674 "March 2016"  686 "March 2017" 698 "March 2018", labsize(med))xmtick(#36) ///
ylabel(0(250) 500,labsize(medlarge)) graphregion(color(white)) bgcolor(white) ytitle("Number of patients", size(medlarge)) ///
xtitle(Date of first CD4 count, size(medlarge)) xline(680, lw(3) lc(gs15)) ///
legend(order(1 "NHLS" 2 "TIER") pos(4) ring(0) region(lc(white))) legend(size(small)) ///
title(KwaZulu-Natal, size(medium))
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\plot_kzn.dta"

**********MP*************

use "D:CD4_ANALYSIS\New_approach_clean firstcd4\first_cd4_tier_vars_30_mp.dta"
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
drop if yearmonth<tm(2015m3)
br
sort yearmonth
br yearmonth
gen first_CD4_tier=1
collapse (sum) first_CD4_tier,by (yearmonth)

gen daily_average= first_CD4_tier/30.5
br
line daily_average yearmonth
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\MP_tier_daily.dta"
***NHLS KZN****
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_nhls_vars_mp.dta"

gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
drop if yearmonth<tm(2015m3)

sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)
gen daily_average= first_CD4_nhls/30.5

ren daily_average daily_average_nhls

save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\MP_nhls_daily.dta"

joinby yearmonth using  "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\MP_tier_daily.dta"
ren daily_average daily_average_tier

save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\MP_nhls_tier_daily.dta"

line daily_average_nhls daily_average_tier yearmonth
*****Graph**********
twoway (line daily_average_nhls  yearmonth) (line daily_average_tier yearmonth), ///
xlabel(662 "March 2015"  674 "March 2016"  686 "March 2017" 698 "March 2018", labsize(med))xmtick(#36) ///
ylabel(0(250) 250,labsize(medlarge)) graphregion(color(white)) bgcolor(white) ytitle("Number of patients", size(medlarge)) ///
xtitle(Date of first CD4 count, size(medlarge)) xline(680, lw(3) lc(gs15)) ///
legend(order(1 "NHLS" 2 "TIER") pos(4) ring(0) region(lc(white))) legend(size(small)) ///
title(Mpumalange, size(medium))
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\plot_mp.dta"

**********LP*************

use "D:CD4_ANALYSIS\New_approach_clean firstcd4\first_cd4_tier_vars_30_lp.dta"
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
drop if yearmonth<tm(2015m3)
br
sort yearmonth
br yearmonth
gen first_CD4_tier=1
collapse (sum) first_CD4_tier,by (yearmonth)

gen daily_average= first_CD4_tier/30.5
br
line daily_average yearmonth
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\LP_tier_daily.dta"
***NHLS LP****
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_nhls_vars_lp.dta"

gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
drop if yearmonth<tm(2015m3)

sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)
gen daily_average= first_CD4_nhls/30.5

ren daily_average daily_average_nhls

save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\LP_nhls_daily.dta"

joinby yearmonth using  "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\LP_tier_daily.dta"
ren daily_average daily_average_tier

save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\LP_nhls_tier_daily.dta"

line daily_average_nhls daily_average_tier yearmonth
*****Graph**********
twoway (line daily_average_nhls  yearmonth) (line daily_average_tier yearmonth), ///
xlabel(662 "March 2015"  674 "March 2016"  686 "March 2017" 698 "March 2018", labsize(med))xmtick(#36) ///
ylabel(0(250) 250,labsize(medlarge)) graphregion(color(white)) bgcolor(white) ytitle("Number of patients", size(medlarge)) ///
xtitle(Date of first CD4 count, size(medlarge)) xline(680, lw(3) lc(gs15)) ///
legend(order(1 "NHLS" 2 "TIER") pos(4) ring(0) region(lc(white))) legend(size(small)) ///
title(Limpop, size(medium))
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\plot_lp.dta"

*********NC*******
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\first_cd4_tier_vars_30_nc.dta"
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
drop if yearmonth<tm(2015m3)
br
sort yearmonth
br yearmonth
gen first_CD4_tier=1
collapse (sum) first_CD4_tier,by (yearmonth)

gen daily_average= first_CD4_tier/30.5
br
line daily_average yearmonth
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\MC_tier_daily.dta"
***NHLS LP****
use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\first_cd4_nhls_vars_nc.dta"

gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2004m1)
drop if yearmonth>tm(2018m3)
drop if yearmonth<tm(2015m3)

sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)
gen daily_average= first_CD4_nhls/30.5

ren daily_average daily_average_nhls

save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\NC_nhls_daily.dta"

joinby yearmonth using  "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\NC_tier_daily.dta"
ren daily_average daily_average_tier

save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\NC_nhls_tier_daily.dta"

line daily_average_nhls daily_average_tier yearmonth
*****Graph**********
twoway (line daily_average_nhls  yearmonth) (line daily_average_tier yearmonth), ///
xlabel(662 "March 2015"  674 "March 2016"  686 "March 2017" 698 "March 2018", labsize(med))xmtick(#36) ///
ylabel(0(30) 30,labsize(medlarge)) graphregion(color(white)) bgcolor(white) ytitle("Number of patients", size(medlarge)) ///
xtitle(Date of first CD4 count, size(medlarge)) xline(680, lw(3) lc(gs15)) ///
legend(order(1 "NHLS" 2 "TIER") pos(4) ring(0) region(lc(white))) legend(size(small)) ///
title(Northern Cape, size(medium))
save "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\plot_nc.dta"

cd "D:CD4_ANALYSIS\New_approach_clean firstcd4\Daily_averages_plots by provinces\"

graph combine  GP.gph KZN.gph MP.gph LP.gph NC.gph

**********Interrupted Time series Anysis Table 2*********************************
use "D:CD4_ANALYSIS\ITS_18MO\NHLS_daily_average_All.dta"
gen postUTT = yearmonth >= tm(2016m9)

reg daily_average_nhls postUTT,r


gen period = 1 if yearmonth < tm(2016m9)
replace period = 2 if yearmonth  >= tm(2016m9) & yearmonth  <= tm(2017m3)
replace period = 3 if yearmonth  >= tm(2017m4) & yearmonth  <=tm(2018m3)

*****Adjusting for pre-trends******
reg daily_average_nhls yearmonth  if period==1, r

predict Y_resids, res

*****entire 18 months post********
regress Y_resids postUTT, r


***************0-6 & 7-18months***********
regress Y_resids i.period, r 
margins period


******nhls plot
reg daily_average_nhls yearmonth if yearmonth < tm(2016m9),r
predict y_hat
predict y_resid, resid
reg y_resid i.period, r
predict y_resid_hat 
margins period
gen fit = y_resid_hat + y_hat

twoway (connected daily_average_nhls yearmonth, ms(9) lw(thin)) ///
	(line y_hat yearmonth if postUTT==0, lc(maroon) lp(solid) lw(medthick)) ///
	(line y_hat yearmonth if postUTT==1, lc(maroon) lp(shortdash) lw(medthick)), ///
	graphregion(color(white)) legend(off) ytitle("Daily Count", size(large)) xtitle("Date", size(large)) ///
	ylabel(,labsize(medlarge) nogrid) xlabel(,labsize(medlarge)) xline(679.5, lp(solid) lc(gs8) lw(med)) 


	
	
**********Tier estimates*****************
use "D:CD4_ANALYSIS\ITS_18MO\Tier_daily_average_All.dta",clear

gen postUTT = yearmonth >= tm(2016m9)

reg daily_average_tier postUTT,r

dis _b[_cons] /* col 2 */
dis _b[postUTT] + _b[_cons] /* col 3 */

dis _b[postUTT] /* col 4 */
dis _b[postUTT] - _se[postUTT]*1.96 /* col 4, lower CI*/
dis _b[postUTT] + _se[postUTT]*1.96 /* col 4, upper CI*/

gen period = 1 if yearmonth < tm(2016m9)
replace period = 2 if yearmonth  >= tm(2016m9) & yearmonth  <= tm(2017m3)
replace period = 3 if yearmonth  >= tm(2017m4) & yearmonth  <=tm(2018m3)

*****Adjusting for pre-trends******
reg daily_average_tier yearmonth  if period==1,r
predict Y_resid, res

*****entire 18 months post********
regress Y_resid i.postUTT, r
margins postUTT


***************0-6 & 7-18months***********
regress Y_resid i.period,r
margins period

*tier plot
reg daily_average_tier yearmonth if yearmonth < tm(2016m9),r
cap drop y_resid 
cap drop y_hat
cap drop y_resid_hat
cap drop fit
predict y_hat
predict y_resid, resid
reg y_resid i.period, r
predict y_resid_hat 
margins period
gen fit = y_resid_hat + y_hat

twoway (connected daily_average_tier yearmonth, ms(9) lw(thin)) ///
	(line y_hat yearmonth if postUTT==0, lc(maroon) lp(solid) lw(medthick)) ///
	(line y_hat yearmonth if postUTT==1, lc(maroon) lp(shortdash) lw(medthick)), ///
	graphregion(color(white)) legend(off) ytitle("Daily Count", size(large)) xtitle("Date", size(large)) ///
	ylabel(,labsize(medlarge) nogrid) xlabel(,labsize(medlarge)) xline(679.5, lp(solid) lc(gs8) lw(med)) 



*********Proportion Tier**********
*use "D:CD4_ANALYSIS\ITS_18MO\Tier_daily_proportion.dta",clear	

use "D:CD4_ANALYSIS\ITS_18MO\Proportion in care_newfeb.dta",clear	
gen period = 1 if month < tm(2016m9)
replace period = 2 if month  >= tm(2016m9) & month  <= tm(2017m3)
replace period = 3 if month  >= tm(2017m4) & month  <=tm(2018m3)

gen postUTT = month >= tm(2016m9)

reg prop_CD4_daily postUTT,r

*******Adjusting for pre-trends******
reg prop_CD4_daily month  if period==1
predict Y_resids, res

*****entire 18 months post********
regress Y_resids postUTT,r
margins postUTT

***************0-6 & 7-18months***********
regress Y_resids i.period,r
margins period

reg  prop_CD4_daily month if month < tm(2016m9),r
predict y_hat
predict y_resid, resid
reg y_resid i.period, r
predict y_resid_hat 
margins period
gen fit = y_resid_hat + y_hat

*******Tier proportion plot*
twoway (connected prop_CD4_daily month, ms(9) lw(thin)) ///
	(line y_hat month if postUTT==0, lc(maroon) lp(solid) lw(medthick)) ///
	(line y_hat month if postUTT==1, lc(maroon) lp(shortdash) lw(medthick)), ///
	graphregion(color(white)) legend(off) ytitle("Daily proportion", size(large)) xtitle("Date", size(large)) ///
	ylabel(,labsize(medlarge) nogrid) xlabel(,labsize(medlarge)) xline(679.5, lp(solid) lc(gs8) lw(med)) 
	
	
*************Daily averages************************
use "D:CD4_ANALYSIS\ITS_18MO\first_cd4_tier_vars_30_All.dta"
gen yearmonth=mofd(resultdate)
format yearmonth %tm
drop if yearmonth<tm(2015m3)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
collapse (sum) first_CD4_tier,by (yearmonth)
br
gen daily_average= first_CD4_tier/30.4
br
line daily_average yearmonth
ren daily_average daily_average_tier


save "D:CD4_ANALYSIS\ITS_18MO\Tier_daily_average_All.dta"

*****************NHLS DATA*********
gen yearmonth=mofd(test_date)
format yearmonth %tm
drop if yearmonth<tm(2015m3)
drop if yearmonth>tm(2018m3)
br
sort yearmonth
br yearmonth
gen first_CD4_nhls=1
collapse (sum) first_CD4_nhls,by (yearmonth)



gen daily_average_nhls= first_CD4_nhls/30.4
br
line daily_average yearmonth


save "D:CD4_ANALYSIS\ITS_18MO\NHLS_daily_average_All.dta""

use "D:CD4_ANALYSIS\New_approach_clean firstcd4\New_plots_daily averages\NHLS_daily_quarter.dta"

joinby yearmonth using "D:CD4_ANALYSIS\ITS_18MO\Tier_daily_average_All.dta"

line daily_average_nhls daily_average_tier yearmonth

twoway (line daily_average_nhls  yearmonth) (line daily_average_tier yearmonth), ///
xlabel(662 "March 2015"  674 "March 2016"  686 "March 2017" 698 "March 2018", labsize(med))xmtick(#36) ///
ylabel(0(250) 1250,labsize(medlarge)) graphregion(color(white)) bgcolor(white) ytitle("Number of patients", size(medlarge)) ///
xtitle(Date of first CD4 count, size(medlarge)) xline(680, lw(3) lc(gs15)) ///
legend(order(1 "NHLS" 2 "TIER") pos(4) ring(0) region(lc(white))) legend(size(small)) 
*title("Number of new patients (per day) with a first CD4 count in NHLS and TIER.net databases", size(small))

twoway (line daily_average_nhls  yearmonth) (line daily_average_tier yearmonth), ///
xlabel(660 "Jan 2015" 672 "Jan 2016"  684 "Jan 2017"  696 "Jan 2018", labsize(small))xmtick(#25) ///
ylabel(100(200) 1200,labsize(small)) graphregion(color(white)) bgcolor(white) ytitle("Daily average number of patients (First CD4 count)", size(small)) ///
xtitle(Date of first CD4 count, size(medlarge)) xline(680, lw(3) lc(gs15)) ///
legend(order(1 "NHLS" 2 "Tier.net") pos(6) col(1) ring() region(lc(white))) legend(size(small)) ///
title(All five Provinces, size(small))

662 "March 2015" 668 "Sept 2015" 674 "March 2016" 680 "Sept 2016" 686 "March 2017" 692 "Sept 2017" 698 "March 2018"

twoway (line prop_cd4 month), ///
xlabel(662 "March 2015"  674 "March 2016"  686 "March 2017" 698 "March 2018", labsize(med))xmtick(#36) ///
ylabel(10(10) 100,labsize(medlarge)) graphregion(color(white)) bgcolor(white) ytitle("Percent of patiients presenting in TIER.Net", size(medlarge)) ///
 xtitle(Date of first CD4 count, size(medlarge)) xline(680, lw(3) lc(gs15)) 

*xtitle(Year month, size(small)) xline(680, lw(3) lc(gs15)) ///

title(All five Provinces, size(small))

****************NHLS_TIER_ENTER_CARE***************

twoway (line daily_average_enter_care yearmonth,lpattern(longdash)) (line daily_average_nhls  yearmonth) (line daily_average_tier yearmonth) , ///
xlabel(662 "March 2015"  674 "March 2016"  686 "March 2017" 698 "March 2018", labsize(med))xmtick(#36) ///
ylabel(0(250) 1500,labsize(medlarge)) graphregion(color(white)) bgcolor(white) ytitle("Number of patients", size(medlarge)) ///
xtitle(Date of first CD4 count, size(medlarge)) xline(680, lw(3) lc(gs15)) ///
legend(order(1 "Enter care" 2 "NHLS" 3 "TIER") pos(4) ring(0) region(lc(white))) legend(size(vsmall)) 



****Adjusting for people entering care*************

use "D:CD4_ANALYSIS\ITS_18MO\Daily_averages_plot_tier_nhls.dta"
gen ratio_nhls_tier= first_CD4_nhls/ first_CD4_tier

keep yearmonth ratio_nhls_tier
save "D:CD4_ANALYSIS\ITS_18MO\adjustiing for entry_into_care.dta"

use "D:CD4_ANALYSIS\ITS_18MO\proportion_EC_CD4_monthly.dta"
ren month yearmonth
 
 joinby yearmonth using "D:CD4_ANALYSIS\ITS_18MO\adjustiing for entry_into_care.dta"
 
 twoway (line Adjusted_prop yearmonth), ///
xlabel(662 "March 2015"  674 "March 2016"  686 "March 2017" 698 "March 2018", labsize(med))xmtick(#36) ///
ylabel(10(10) 100,labsize(medlarge)) graphregion(color(white)) bgcolor(white) ytitle("Percent of patiients presenting in TIER.Net", size(medlarge)) ///
 xtitle(Date of first CD4 count, size(medlarge)) xline(680, lw(3) lc(gs15)) 
 
 save "D:CD4_ANALYSIS\ITS_18MO\Adjusted proportions in care.dta"
 
 *************Kernell distribution comaprison************
 twoway kdensity test_result if UTT_period == 1 & test_result<1500, lcolor(blue) lpattern(solid) ///
        || kdensity test_result if UTT_period ==0  & test_result<1500, lcolor(red) lpattern(dash) ///
        title("CD4 Count Distribution: Pre-UTT vs. Post-UTT") ///
        ylabel(0(0.001)0.003) ///
        legend(label(1 "Pre-UTT") label(2 "Post-UTT"))
		
		
twoway (kdensity CD4_count if year == 2015, lcolor(blue) lpattern(solid)) ///
       (kdensity CD4_count if year == 2016, lcolor(red) lpattern(dash)) ///
       (kdensity CD4_count if year == 2017, lcolor(green) lpattern(dot)) ///
       title("CD4 Count Distribution: 2015, 2016, 2017") ///
       xlabel(0(200)1500) ylabel(0(0.001)0.004) ///
       legend(label(1 "2015") label(2 "2016") label(3 "2017"))
	   
	   
	   twoway (kdensity CD4_count if test_year==2015 & CD4_count<=1500, lcolor(blue) lpattern(solid)) ///
       (kdensity CD4_count if test_year == 2016 & CD4_count<=1500, lcolor(red) lpattern(dash)) ///
       (kdensity CD4_count if test_year == 2017 & CD4_count<=1500, lcolor(black) lpattern(short_dash)), ///
       title("CD4 Count Distribution: 2015, 2016, 2017 (NHLS)") ///
       xlabel(0(200)1500) ylabel(0(0.001)0.002) ///
       legend(label(1 "2015") label(2 "2016") label(3 "2017"))
	   
	   
	    twoway (kdensity CD4_count if test_year==2015 & CD4_count<=1500, lcolor(blue) lpattern(solid)) ///
       (kdensity CD4_count if test_year == 2016 & CD4_count<=1500, lcolor(red) lpattern(dash)) ///
       (kdensity CD4_count if test_year == 2017 & CD4_count<=1500, lcolor(black) lpattern(short_dash)), ///
       title("CD4 Count Distribution: 2015, 2016, 2017 (TIER.NET)") ///
       xlabel(0(200)1500) ylabel(0(0.001)0.002) ///
       legend(label(1 "2015") label(2 "2016") label(3 "2017"))
