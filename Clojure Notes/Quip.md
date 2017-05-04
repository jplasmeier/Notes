# Quip

A simple command-line utility for messing with `.csv` files. 

### Design


### User Interface

Give a range of columns and a range of rows and return it. 

```
street,city,zip,state,beds,baths,sq__ft,type,sale_date,price,latitude,longitude3526 HIGH ST,SACRAMENTO,95838,CA,2,1,836,Residential,Wed May 21 00:00:00 EDT 2008,59222,38.631913,-121.43487951 OMAHA CT,SACRAMENTO,95823,CA,3,1,1167,Residential,Wed May 21 00:00:00 EDT 2008,68212,38.478902,-121.4310282796 BRANCH ST,SACRAMENTO,95815,CA,2,1,796,Residential,Wed May 21 00:00:00 EDT 2008,68880,38.618305,-121.4438392805 JANETTE WAY,SACRAMENTO,95815,CA,2,1,852,Residential,Wed May 21 00:00:00 EDT 2008,69307,38.616835,-121.4391466001 MCMAHON DR,SACRAMENTO,95824,CA,2,1,797,Residential,Wed May 21 00:00:00 EDT 2008,81900,38.51947,-121.4357685828 PEPPERMILL CT,SACRAMENTO,95841,CA,3,1,1122,Condo,Wed May 21 00:00:00 EDT 2008,89921,38.662595,-121.3278136048 OGDEN NASH WAY,SACRAMENTO,95842,CA,3,2,1104,Residential,Wed May 21 00:00:00 EDT 2008,90895,38.681659,-121.351705
```
_sample csv file_

`quip -cols 0:3,4,6 -rows 0:2`
returns

```
street,city,zip,state,beds,sq__ft3526 HIGH ST,SACRAMENTO,95838,CA,2,83651 OMAHA CT,SACRAMENTO,95823,CA,3,1167
```

(define process-rows
  (lambda (row row-ctr)
    (conj row row-ctr)))
    
(define index-rows
	(lambda (start end row-ctr)
	(subvec row-ctr start end)))
	
### Implementation

Or, Bad Ideas

Using `doall`.

