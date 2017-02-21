#HOW TO CREATE A PRECINCT ELECTION MAP?

##DEFINITIONS
- GeoID = a global id for every polygon on Datamap (e.g. 84006 for California)
- EXT = External Data
- REF = Map Reference File (features from the Map combined with relevant external data)
- MAP = TopoJSON (mostly derived from a shapefile)
- DATA = EXT + REF combined
- VIZ

##Mapping Election Data by Precinct

###STEP 1: EXT
- Find and download General Election data directly from the county. Make sure that the results are official. Corroborate with State Results.
- Bring the Election Data in a suitable form to merge with the REF respectively to map (e.g. follow the precinct EXT template)

###STEP 2: REF
- Find the precint map of the county as Shapefile. Check with the county if the shapefile is still up to date.
- Export the feature data to csv => Base for your REF file
- Create GeoID

###STEP 3: MAP
- Transform Shapefile into TopoJSON
- Add GeoID, remove all other features

###STEP 4: DATA
- Merge REF and EXT and you get the data

###STEP 5: VIZ 1
- Use the precinct VIZ template (choropleth) in d3 to map DATA to color

###STEP 6: VIZ 2
- Add additonal geo layers (city names;  place boundaries, e.g. townships; other like Bay Area, leaflet)



##Example:

###1. EXT 
http://traviselectionresults.com/enr/archives/display.do?criteria.electionId=20121106&electionId=20121106&tabType=C

http://traviselectionresults.com/enr/results/display.do?criteria.electionId=201611&electionId=0&tabType=C&criteriaId=-1&formSubmitted=1
```
Statistics for All Votes Cast in Travis County 247 Precincts	
194 of 194 (100.00% EDay locations counted)
VIEW EDAY LOCATIONS MAP
Total Registered Voters in Travis County:	732,037	 	Total Ballots Cast in Travis County:	477,588	65.24%
Early Voting Turnout:	51.10%	 	Early Voting Ballots Cast:	374,052	78.32%
Election Day Turnout:	14.14%	 	Election Day Ballots Cast:	103,536	21.68%
```
Overview
```
PRESIDENT
 										Early Voting	 	Election Day	 	Total Votes
Donald J. Trump / Mike Pence (REP)		97,708	26.55%	 	29,501	29.29%	 	127,209	27.14%
Hillary Clinton / Tim Kaine (DEM)		248,501	67.53%	 	59,759	59.33%	 	308,260	65.77%
Gary Johnson / William Weld (LIB)		14,494	3.94%	 	7,464	7.41%	 	21,958	4.68%
Jill Stein / Ajamu Baraka (GRN)			4,653	1.26%	 	2,809	2.79%	 	7,462	1.59%
Darrell L. Castle / Scott N. Bradley	99		0.03%	 	51		0.05%	 	150		0.03%
Scott Cobbler / Michael Rodriguez		14		0.00%	 	4		0.00%	 	18		0.00%
Cherunda Fox / Roger Kushner			2		0.00%	 	0		0.00%	 	2		0.00%
Tom Hoefling / Steve Scholin			26		0.01%	 	21		0.02%	 	47		0.01%
Laurence Kotlikoff / Edward Leamer		69		0.02%	 	19		0.02%	 	88		0.02%
Jonathan Lee / Jeffrey Erskine			0		0.00%	 	3		0.00%	 	3		0.00%
Michael A. Maturen / Juan A. Munoz		91		0.02%	 	40		0.04%	 	131		0.03%
Evan McMullin / Nathan Johnson			2,316	0.63%	 	1,025	1.02%	 	3,341	0.71%
Monica Moorehead / Lamont Lilly	      	6		0.00%	 	5		0.00%	 	11		0.00%
Robert Morrow / Todd Sanders			12		0.00%	 	9		0.01%	 	21		0.00%
Emidio Soltysik / Angela Walker			5		0.00%	 	3		0.00%	 	8		0.00%
Dale Steffes / Paul E. Case				4		0.00%	 	1		0.00%	 	5		0.00%
Tony Valdivia / Aaron Barriere			5		0.00%	 	1		0.00%	 	6		0.00%
Write In								0		0.00%	 	0		0.00%	 	0		0.00%
Total									368,005	 	 		100,715	 	 		468,720	 
   	 
247 Precincts
   	
Total Race Participation: 468,720 of 477,588 total votes cast or 98.14%
```
###Corroborate with State 
http://elections.sos.state.tx.us/elchist319_race62.htm          
http://elections.sos.state.tx.us/elchist319_county227.htm         
Seems to be ok!        

Always good to check State Results too:        
http://elections.sos.state.tx.us/elchist319_state.htm          
So this is correct too!


Here is also a nice precinct map:            
http://traviselectionresults.com/enr/contest/display.do?criteria.electionId=201611&contestId=3&districtId=111111&criteriaId=-1&showAllPrecincts=1&precinctsSelectedStr=101              

Here is even a map where to vote:            
http://traviselectionresults.com/enr/locations/display.do?criteria.electionId=201611&electionId=201611&tabType=C         


####1.1. Excel Download for manual inspection
http://traviselectionresults.com/enr/reports/display.do?criteria.electionId=201611&electionId=201611&tabType=C

[Excel only for President](https://github.com/datamapio/story/blob/master/election/travis/TravisCountyElections_canvass_201611-140--2100195909.xls)

####Precinct Template Form in Wide Form (ideal for Map Visualizations, till we find a better way. Still open to debate)
Wide and Long Form, see: https://stanford.edu/~ejdemyr/r-tutorials/wide-and-long/              

General Template:
```
## CASE A: only totals are available
## precinct_id | precinct_name | registered | ballots_cast | turnout | valid | clinton | trump | 
## 1001        | precint 1001  |        100 |           80 |   80.00 |    70 |      35 |    35 |       
## 1002        | precint 1002  |        200 |          160 |   80.00 |   150 |      80 |    70 |          
##
## CASE B: totals and subtotals are available (elecday, vbm, early, absentee)
## precinct_id | precinct_name | registered | ballots_cast | turnout | valid | clinton | clinton_elecday | clinton_vbm | clinton_early |
## 1001        | precint 1001  |        100 |           80 |   80.00 |    70 |      35 |              25 |           5 |             5 |
## 1002        | precint 1002  |        200 |          160 |   80.00 |   150 |      80 |              50 |          20 |            10 |
```

Applied to Travis Data, you will have:     
                     
Name of future file: EXT_MASTER_presidential_travis_precinct_20161121.csv              
EXT_MASTER = file with all the data,
EXT_ = EXT BASE, the file only with totals.   
There should be 247 rows/observations            
        
```
## precinct_id | precinct_name | registered | ballots_cast | turnout | valid | trump_vbm | trump_early | trump_elecday | trump |
## 101         | Precint 101   |       5622 |         3337 |     	 |       |        21 |         326 |           180 |   527 |
```
- precinct_id = Precinct (XLS)
- precinct_name = "Precinct " + Precinct (XLS)
- registered = Registered Voters (XLS)
- ballots_cast = Total Ballots (XLS)
- turnout = ballots_cast / registered * 100; most of the time available
- valid = candidates and write-ins, but without under- and overvotes; most of the time this is available
- trump = total of vbm, early and elecday 
  (we use the last name of the presidential candidate or last name plus first letter of first name in case of confusion)
- writein = write-in
- overvote, undervote


####1.2. Excel Download in R
We do all this in R (including the download onwards), so that it is reproducible and it is easier to correct data.



###2. REF
Check: Precinct Summary
http://www.traviscountyclerk.org/eclerk/Content.do?code=E.40
Via: http://www.votetravis.com/vexpress/display.do

+ CSV Export from Precinct Map 2012 with Mapshaper


###3. MAP
https://tax-office.traviscountytx.gov/voters/jurisdiction-maps

[PDF](https://tax-office.traviscountytx.gov/images/tax_assessor/pdfs/voters/maps/VR_TravisCountyPrecincts.pdf)
[Shapefile 2012 via Google](https://groups.google.com/forum/#!msg/openaustin/05WFo9uUo_E/peyQp3zohrEJ)       


###4. DATA

###5. VIZ
We use the following template:
https://elections.datamap.io/us/2016/11/08/presidential/precinct/by_state/california/by_county/alameda

####5.1. d3.js
####5.2. Color palette
Starting point: http://colorbrewer2.org/        
https://gka.github.io/palettes/       

See also:              
For the problem of city, county, state names over colors (e.g. https://elections.datamap.io/us/2016/11/02/electoral_college_forecast)
https://toolness.github.io/accessible-color-matrix/?n=white&n=light&n=bright&n=medium&n=dark&n=black&v=FFFFFF&v=B3EFFF&v=00CFFF&v=046B99&v=1C304A&v=000000 and https://brand-html-color-matrix.apps.cloud.gov/visual-style/#colors     

####5.3. Scales
https://github.com/d3/d3-scale             
https://github.com/d3/d3/blob/master/API.md#scales-d3-scale



##Misc 
See also:     
https://github.com/open-austin/voteatx-app             




