#Datamap Culture

##Datamap
Datamap is a cognitive medium – a media environment to support and enable (spatial) thought.
It becomes part of the way users think about spatial, data and visual relationships, and expands the range of ideas they can have.
Datamap augments the human intellect*.           
* See Douglas Engelbart's Augmenting Human Intellect: A Conceptual Framework, and Alan Kay and Adele Goldberg's Personal Dynamic Media.
See: http://cognitivemedium.com/emm/emm.html        


##First Principles

###1. Map Accuracy through Time
Datamap strives to create the most accurate area unit maps over time.
This means that we judge every map or map visualization by its map accuracy first.     
This means that the map together with the reference file (REF) are Datamap's "ground truth".    
Everything following: external data, the visualization etc. comes second. 
When judging a map the following questions should be raised:   
- What is the source?
- What is the date?
- Have we properly researched it? Have we compared different sources (e.g. election data and map data on a given date)?
- Do we properly understand the components of the map (e.g. municipalities, neighboring countries, lakes)?
- Do we know that the map is accurate for the time we need it?
- Do we have a REF or reference file available? If not, do this first

###2. Reproducibility
The download and the cleaning, wrangling of any external data has to be done programmatically, so that anyone can reproduce it. 
We use R for this today, but this might change in the future.

###3. Transparency & Communication
Communicate your findings in a README.md.
Explain your choices especially when there is no simple right and wrong.
Be transparent about processes, understandings and choices all the time.

###4. Text based, human readable
Be it data, maps, visualizations: they are text based and human readable.   

This means:
- GeoJSON and TopoJSON for maps
- CSV for data
- D3 for visualizations

###5. GeoID
All maps use global identifiers, so called GeoIDs (to keep the map as small as possible).
The construction of Datamap's GeoIDs can be checked on [Github](https://github.com/datamapio/geoid/blob/master/lookup.md).
Changes, problems have to be communicated to the GeoID committee.

###6. Separation of concerns
There are three distinct areas:
- MAP area
- DATA area
- VIZ area  

Treat each one separately.            
As a end user you should never need to work on the map, but always connect your external data to the REF.

####MAP
There is one or more layers of maps, e.g. 
- States as shapes   
- Counties as shapes
- Cities as points (with geoid and lat/long)
- Cities as shapes
- Rivers and Lakes
- Major transportation routes

Keep them separate, so that you have a separation of concerns here as well.     
This is important when one layer changes, while the others stay the same (e.g. county shapes change, state shape stays the same).   


####DATA: REF
Each Map layer has a correspondent REF layer. The REF file is the "ground truth".         
The REF contains the GeoID to link to the shapes/area units on the map.             
The REF contains additional information like names and other relationships (eg. to what State a county belongs).            
The REF contains also common matching codes, so that external data (EXT) can be easily merged with the REF.                   
Ideally there is one REF file, but if there are many language version for example, there could be several, identified by their ISO language code.

####DATA: EXT
External data (= EXT) is first processed (cleaned, wrangled) to fit the REF file. 
The REF file helps to understand the difference between EXT and REF.      
Once the external is cleaned and renamed, it can be merged with the REF File.

####DATA = MERGER OF REF AND EXT
The data is the merged REF and EXT file. Via the GeoID the data can be linked up with the MAP.

####VIZ 
The visualization is the last and final step. We try to use a modular approach as much as possible.        
Create a collection of common map types.        
Things like tooltips and legends (see the excellent d3.legend) should also be reuseable.      


## Process
1. Choose a map
2. Reference or Create the GeoID (get an understanding of the space first before creating one)
3. Create the REF file (reference file), publicly available.     
   Explain in a README.md how  
4. Create & Store GeoJSON with GeoIDs    
5. Name it properly (location - area unit - date/time)   
5. Create the TopoJSON
6. Every public map needs to be verified and approved by at least one additional person.     
   Approval processes have to be in written form.  





