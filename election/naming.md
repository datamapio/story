#Naming

We want to create a vocabulary and abbreviations for elections and votes, so that it is easy for people to lookup uncommon abbreviations.
This goes from keeping variable/column names as small as possible (e.g. prop_a_yes_pct or even a_yes_pct) to identify party names internationally (e.g. ch_fdp or us_dem).


The first and canonical version is in english, translations in other languages are highly welcome.


##Data Process
1. Import EXT 
2. Tidy EXT in wide format (see: [tidy](http://vita.had.co.nz/papers/tidy-data.pdf) and [long and wide format](http://www.cookbook-r.com/Manipulating_data/Converting_data_between_wide_and_long_format/))        
   - TIDY_EXT_...(what is recommended)
   - WIDE_EXT... (what we use)
3. Merge EXT and REF = DATA (= WIDE_DATA)
4. Transform DATA
5. SMALL DATA (drop unused columns)


In more detail:       
Point 2. Tidy EXT    
- We use the wide format, not the long format. It would be fully tidy if we had "candidate" and "votes", but that would mean we have the precinct_id multiple times. Question is: should we always make a tidy version too? And             
  http://www.prometheusresearch.com/good-data-management-practices-for-data-analysis-tidy-data-part-2/              
- cleaned data         
- with no added columns (eg.  no % column when there is no % column in the EXT)       

Point 4. Transform DATA       
- Computed Variables / Columns (e.g. %)   
- Computed Observations (e.g. Totals = vbm and elec_day together)
- Subsetting the data (take only a sample)
- Normalizing data (divide car accidents by population)

Point 5. SMALL DATA     
- Drop columns (when not needed in the visualization)      
Maybe this can be automated. The columns not used get highlighted and one can drop them individually or all together? (edited) 



##Column/Variables Names in csv
With Real Life Examples

###Proposal

####EXT -> DATA (before merge with REF)
#####Variables
- precinct_id / fips / gdenr / gdename
- precinct_name
- precinct_split_name (=split_name)
- precinct_split_id
- parent_precinct_id (see SF)
- neighborhood (there might be other boundaries like congressional districts etc)
- contest_id
- contest_title (e.g. PRESIDENT AND VICE PRESIDENT)
- choice_id
- choice_name (Yes or No)
- choice_party
- candidate_id
- candidate_name (or candidate)
- candidate_type (e.g. C (candidate) or W (write-in))
- candidate_party
- registered
- ballots_cast = ballots_cast are always ballots cast in total!               
  Otherwise use: ballots_cast_vbm = ballots cast, vote by mail; ballots_cast_elecday = ballots cast on election day
- turnout (always in %)
- valid = valid ballots or valid votes (without under- and overvotes, but with write-ins)
- writein = write-in, but written together
- undervote
- overvote
- type: vbm (Vote by Mail, Absentee Votes), elecday (Election Day, Election_Votes), early (Early_Votes)
- votes
- reporting (e.g. 1, seen in San Mateo)

For humans use candidate_name (Hillary Clinton), for props use choice_name (Yes or No)        
For TIDY_WIDE, use last name, see guidelines


Abbreviations:
- pct for percentage (PCT only used in GeoID)
- ini for Initiative
- ref for Referendum
- prop for Proposition
- measure for Measure

#####Guidelines
- one csv per measure or prop or referendum or initiative (try to use words used)
  Eg. prop_f (Proposition F), measure_g (Measure G), ini_m (Iniatiative M), ref_b (Referendum B) etc.  
- If the names are long, create an extra sheet to translate: e.g. 
  ini_m = mariage initative = Volksinitiative «Für Ehe und Familie - gegen die Heiratsstrafe» ; 
- Always use precinct_id, registered, ballots_cast, turnout to individual csv's
- Some will have vote by mail, some don't. ballots_cast is always the total
- Some will show under- and overvote, some don't     
- Use last name of candidates (if possible confusion, use last name plus first name initial or full first name)


####DATA (after merge with REF)
- Add sum's and % columns as needed
- Create subsets if needed

#####Guidelines
- Sum up all candidates and write-ins, do not count under- and overvotes, to get to % of candidates


###Transformed EXT (with percentages)
[Alameda Separated](https://github.com/datamapio/US/blob/master/precinct/california/alameda/EXT/alameda_precinct_presidental_separated_20161122.csv)
[Alameda Totals](https://github.com/datamapio/US/blob/master/precinct/california/alameda/EXT/alameda_precinct_presidental_total_20161122.csv)
```
precinct_id 
registered_total  
ballots_cast_total  
turnout_total 
clinton_total 
johnson_total 
lariva_total  
stein_total 
trump_total 
writein_total 
overvote_total  
undervote_total 
clinton_total_pct 
johnson_total_pct 
lariva_total_pct  
stein_total_pct 
trump_total_pct 
writein_total_pct 
overvote_total_pct  
undervote_total_pct
```

###Tidy EXT (without percentages)
[Amador County](https://github.com/datamapio/US/blob/master/precinct/california/amador/EXT/amador_precinct_presidental_total_20161206.csv)     
```
precinct_id 
registered  
ballots_cast  
turnout 
stein 
clinton 
lariva  
trump 
johnson 
writein 
cand_with_writein (summary)
```

San Francisco
```
##id                pct_2012    precinct_names  neighborhood    yes_percentage  parent_id
##84006075PCT1106   1106        Pct 1106/1107   INGLESIDE       0.43            NA
##84006075PCT1107   1107        NA              INGLESIDE       NA              84006075PCT1106
```

San Mateo
```
[1] "Precinct_Name"    "Split_Name"       "precinct_splitId" "Reg_voters"       "Ballots"         
[6] "Reporting"        "Contest_id"       "Contest_title"    "Contest_party"    "Choice_id"       
[11] "Candidate_name"   "Choice_party"     "Candidate_Type"   "Absentee_votes"   "Early_votes"     
[16] "Election_Votes" 
```


###Switzerland

From the Swiss Statistics Office:
```
## X.1 = Gemeindenummer (gdenr)
## X.2. = Gemeindename (gdename)
## X.3 = Stimmberechtigte = entitled_to_vote / registered
## X.4 = Abgegebene Stimmen = votes_cast / ballots_cast
## X.5 = Stimmbeteiligung = turnout
## X.6 = Gültige Stimmen = valid_votes / valid 
## X.7 = JA-Stimmen = yes
## X.8 = NEIN-Stimmen = no
## X.9 = JA-Stimmen in Prozent = yes_percentage
```

From Canton Bern
Propositions / Ballot Measures
```
# m = mariage = Volksinitiative «Für Ehe und Familie - gegen die Heiratsstrafe» ; 
# e = enforcement = Volksinitiative «Zur Durchsetzung der Ausschaffung krimineller Ausländer (Durchsetzungsinitiative)» 
# f = food = Volksinitiative «Keine Spekulation mit Nahrungsmitteln!» 
# r = road = Änderung des Bundesgesetzes über den Strassentransitverkehr im Alpengebiet (Sanierung Gotthard-Strassentunnel)

#Verwaltungskreis;Stimmber.;Stimmbet.; Ja;Nein;Ja;Nein;             Ja;Nein;Ja;Nein;             Ja;Nein;Ja;Nein;            Ja;Nein;Ja;Nein ;    
# Biel/Bienne; 62'590; 55.9%;            15'881;18'399;46.3%;53.7%;   12'854;22'116;36.8%;63.2%;   15'888;17'948;47.0%;53.0%;   19'456;15'036;56.4%;43.6% 

Variable names in the CSV:    
"municipality_name", "entitled_to_vote","turnout", "yes_m", "no_m", "yes_m_pct", "no_m_pct", "yes_e", "no_e", "yes_e_pct", "no_e_pct", "yes_f", "no_f", "yes_f_pct", "no_f_pct", "yes_r", "no_r", "yes_r_pct", "no_r_pct")
```


From Elex
http://elex.readthedocs.org/en/latest/cli.html
```
commands:

  ballot-measures
    Get ballot positions (also known as ballot issues)

  candidate-reporting-units
    Get candidate reporting units (without results)

  candidates
    Get candidates

  delegates
    Get all delegate reports

  elections
    Get list of available elections

  next-election
    Get the next election (if date is specified, will be relative to that date, otherwise will use today's date)

  races
    Get races

  reporting-units
    Get reporting units

  results
    Get results

positional arguments:
  date                  Election date (e.g. "2015-11-03"; most common date
                        formats accepted).

optional arguments:
  -h, --help            show this help message and exit
  --debug               toggle debug output
  --quiet               suppress all output
  -o {json,csv}         output format (default: csv)
  -t, --test            Use testing API calls
  -n, --not-live        Do not use live data API calls
  -d DATA_FILE, --data-file DATA_FILE
                        Specify data file instead of making HTTP request when
                        using election commands like `elex results` and `elex
                        races`.
  --delegate-sum-file DELEGATE_SUM_FILE
                        Specify delegate sum report file instead of making
                        HTTP request when using `elex delegates`
  --delegate-super-file DELEGATE_SUPER_FILE
                        Specify delegate super report file instead of making
                        HTTP request when using `elex delegates`
  --format-json         Pretty print JSON when using `-o json`.
  --results-level       Specify reporting level when using `elex results`, such as 'district' or 'state'
  -v, --version         show program's version number and exit
```



##Proposal

###Percentage
Abbreviation: pct (not to confound with PCT (=Precinct) in GeoID)    
Variable Suffix: _pct    
Example: prop_r_yes_pct = Proposition R, Yes Percentage     


###Ballot Measure, Proposition, Prop, Initiative, Referendum
https://en.wikipedia.org/wiki/Ballot_measure   
Giving a proposition a letter of the alphabet is easy, but how to differentiate between different props (local, national)?
In Switzerland, there are often national, cantonal, district and municipality props on the agenda.
Or see California: https://ballotpedia.org/Notable_local_measures_on_the_ballot#California

###Eligible to vote, Entitled to Vote, Registered to vote
http://www.statisticbrain.com/voting-statistics/
```
Total number of Americans eligible to vote	218,959,000
Total number of Americans registered to vote	146,311,000
Total number of Americans who voted in the 2012 Presidential election	126,144,000
Percent of Americans who voted in the 2012 Presidential election	57.5 %
State with the highest voter turnout rate (Minnesota)	75 %
State with the lowest voter turnout rate (Utah)	53.1 %
```
entitled to vote = registered to vote (in some countries you have to register first, in others you are entitled to vote by default)



###Voter Turnout, Voter Turnout Rate




##See also
http://ucsd.libguides.com/c.php?g=90733&p=584828
http://www.globalelectionsdatabase.com/index.php/links






