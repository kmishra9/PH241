

## WASH Benefits Bangladesh primary outcomes analysis datasets

Updated 1 Feb 2018

The WASH Benefits Bangladesh trial enrolled 5,551 compounds in rural Bangladesh. Compounds were distributed across 720 clusters, which in turn were randomized in 90 groups of 8 geographically contiguous clusters to one of either 6 intervention arms or a double-sized control arm. The study is a multi-treatment version of a pair-matched design, with 90 “pairs” of 8-tuples. Compounds were eligible for enrollment if there was at least one pregnant woman in her second trimester. We measured primary outcomes, length-for-age Z-scores (LAZ), after 2 years of intervention, when children were a median of 22 months old (10th to 90th percentile: 20-25 months). We additionally collected outcome measurements after one year of intervention, when children were a median of 9 months old (10th to 90th percentile: 6.5-11 months). At both visits, we collected caregiver reported diarrhea for index children in the birth cohort, and all other children who were <36 months at enrollment. We also collected measures of intervention adherence (uptake).  For additional details, please see the trial’s study design and rationale article [http://bmjopen.bmj.com/content/3/8/e003476.short](http://bmjopen.bmj.com/content/3/8/e003476.short).

This directory provides cleaned datasets used in the primary analysis of the Bangladesh trial for anthropometric outcomes (length, weight, weight-for-height), diarrhea, and all-cause mortality. Below is a brief summary of each dataset. Please refer to the Stata-generated codebooks for variable details and additional metadata.  All publicly available datasets have a `-public` suffix.  To run analysis scripts you will need to point them to the new data file names.  In public datasets the ID variables have been recoded to break any link to publicly identifiable information.  The ID variables have been recoded in a consistent way so that public versions of the WASH Benefits Bangladesh trial data should link across all files.  Should you have questions about the data, the best person to contact is Ben Arnold at the University of California, Berkeley (benarnold@berkeley.edu). A backup contact is Jack Colford (jcolford@berkeley.edu).

The scripts that complete all analyses using these data are publicly available through [Github](https://github.com/ben-arnold/WBB-primary-outcomes) and the [Open Science Framework](osf.io/63uzb).

### Treatment assignments (washb-bangladesh-tr)

This file includes 720 records -- one row for each cluster enrolled in the study. Clusters were grouped by 8 into 90 randomization blocks (matched blocks). Since the matching was done geographically, clusters within the same block are proximal to one another -- they are separated by a minimum of 1 km (median = 2.6 km; IQR = 1.8, 3.7 km). However, it is not safe to assume that sequential blocks are geographically proximal to one another -- that is true in some cases but not in general.  This file can be merged to any of the other analysis files by `clusterid`. Note that there are two versions of this file, one with the real treatment assignments  (`washb-bangladesh-tr`) and a second with re-randomized, blinded treatment assignments (`washb-bangladesh-blind-tr`). The blinded version of the file enabled our internal, blinded replication before the study was unmasked.

### Enrollment characteristics (washb-bangladesh-enrol)

This file includes 5,551 records -- one row for each compound enrolled in the trial. The variables we included are based on those needed to compare arms for balance in their baseline characteristics, as well as characteristics we pre-specified to include in adjusted analyses of intervention effects. There is a single variable, maternal height (`momheight`), which was not measured at enrollment -- instead, measured at year 1 and year 2 follow-up visits. We opted to measure maternal height in later visits because we felt that it could have been influenced by pregnancy.  

### Compound tracking (washb-bangladesh-track-compound)

This file provides tracking information for the 5,551 compounds enrolled at the year 1 and year 2 visits. It includes a single observation per compound and is the basis for tracking CONSORT study population flow in the trial. Note that in the 30 cases of twins born in the study, in no case did they have differential loss-to-follow-up within a compound, and in no case did an index child who was a twin die during the study. Thus, tracking loss-to-follow-up and mortality at the compound level is parsimonious and valid. The file includes 4 variables that record the compounds status at the year 1 and 2 visits (`miss1` indicates if they were missing, and `miss1reason` classifies the reason; `miss2` and `miss2reason` provide the same information for year 2).  Note that if a compound's status is `withdrew`, that means the field team interacted with the study compound but they requested to exit the trial. If their status was `absent`, that means that the field team was unable to touch base with the study compound but the enrolled participants still lived in the same compound (to our knowledge).

### Adherence (washb-bangladesh-uptake)

This file includes 14,906 records that are measured at the level of compound and measurement time (enrollment, year 1, year 2). Observations are uniquely identified by `dataid` and `svy`.  At enrollment, there were 5,551 compounds and due to attrition in the trial this number decreased to 4,716 in year 1 and 4,639 in year 2. There are two different measures of LNS consumption based on counting sachets present at the time of a field visit (`lnsn`, `lnsp`) and on the number of sachets in the past week reported by the mother (`rlnsn`, `rlnsp`). The LNS consumption is estimated or reported per week, so the expected number of sachets for 100% adherence is 7 days x 2 sachets per day = 14. Values of `lnsn` that are negative or >28 are implasible (measurement error). The trial used the `rlnsp` variable in the peer reviewed article.


### Anthropometry measurements (washb-bangladesh-anthro)

The study measured length, weight, and head circumference from 4,708 index children at the year 1 follow-up and 4,633 index children at the year 2 follow-up. The 9,341 observations are uniquely identified by compound ID (`dataid`), child ID (`childid`) and survey round (`svy`). The WASH Benefits common module 4 (located in this same OSF repository: https://osf.io/gejux/) includes details of the anthropometry measurement and training protocol -- we followed standard protocols adopted from the iLiNS trials ([ilins.org](ilins.org)) and FANTA standards [1,2]. Note, however, that our original protocol called for 3 measurements on all children, but that during the final field activities the team modified the protocol such that children were measured 2 times and if the measurements differed by >0.1 cm (length) or >0.1 kg (weight) anthropometrists collected a third measurement.  The file includes raw measurements for weight, length, and head circumference, as well as their median. We used median values to compute Z-scores based on the WHO 2006 growth standards. We used the Stata package zscore06 to compute LAZ, WAZ, and WHZ scores, and the WHO igrowup macro to compute HCZ scores (note: we confirmed that zscore06 and igrowup produced identical Z scores for the three in which they overlapped). Z-scores beyond the WHO ranges of biologic plausibility (<0.3% of measurements) have been set to missing and are flagged with a corresponding indicator variable (e.g., `laz_x` indicates extreme values for `laz`).


### Diarrhea measurements (washb-bangladesh-diar)

The study measured diarrhea from index children (also called “target children”) in the birth cohort along with other children living in the compound who were <36 months at enrollment. The 22,594 observations are uniquely identified by compound ID (`dataid`), child ID (`childid`) and survey round (`svy`). The variable `tchild` indicates target children (referred to as "index children" in the manuscript) and others (note that `tchild` takes on two values: `Target child` and `Sibling`, where sibling is a bit of a misnomer because it includes true siblings along with age-eligible children from other families in the same compound). Older children were also measured at enrollment (target children were still in utero at that time).

We defined diarrhea as ≥3 loose or watery stools in 24 hours OR  ≥1 stool with blood using a 7-day recall period [3]. We also measured symptoms using a 2-day recall period (also provided in this dataset), but used a 7-day recall period after confirming there was no evidence for differential recall by arm to improve power [4], as specified in our protocol. The `diar7d` variable is the variable used in the primary analysis. The dataset also includes two negative control outcomes [5,6,7] -- toothache and bruising/abrasions -- which we measured to detect potentially differential bias in symptom reporting since the trial was unblinded (note: we found no evidence for differences between arms in these negative control outcomes).


### References

[1] Cogill B. Anthropometric indicators measurement guide. Washington, D.C.: Food and Nutrition Technical Assistance Project, Academy for Educational Development; 2003.

[2] de Onis M, Onyango AW, den Broeck JV, Chumlea WC, Martorell R. Measurement and standardization protocols for anthropometry used in the construction of a new international growth reference. Food Nutr Bull. 2004;25: S27–S36.

[3] Baqui AH, Black RE, Yunus M, Hoque AR, Chowdhury HR, Sack RB. Methodological issues in diarrhoeal diseases epidemiology: definition of diarrhoeal episodes. Int J Epidemiol. 1991;20: 1057–1063.

[4] Arnold BF, Galiani S, Ram PK, Hubbard AE, Briceno B, Gertler PJ, et al. Optimal Recall Period for Caregiver-reported Illness in Risk Factor and Intervention Studies: A Multicountry Study. Am J Epidemiol. 2013;177: 361–370.

[5] Lipsitch M, Tchetgen ET, Cohen T. Negative controls: a tool for detecting confounding and bias in observational studies. Epidemiology. 2010;21: 383–388.

[6] Arnold BF, Ercumen A, Benjamin-Chung J, Colford JM Jr. Negative controls to detect selection bias and measurement bias in epidemiologic studies. Epidemiology. 2016;27: 637–641.

[7] Arnold BF, Ercumen A. Negative Control Outcomes: A Tool to Detect Bias in Randomized Trials. JAMA. 2016;316: 2597–2598.
