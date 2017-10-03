# PostgreSQL (OSX specific)

## Postgres and PostGIS = Postgres.app
https://postgresapp.com/documentation/

## GUI Client Apps
https://www.pgadmin.org/
https://eggerapps.at/postico/
http://www.macpostgresclient.com/SQLProPostgres

## psql: Import csv into a table (command line)
Can be started from Postgres.app
Important if you want to copy a csv file into your table

```
copy ch01.restaurants_staging
 FROM '/data/restaurants.csv' DELIMITER as ',';
```
Here we copy the restaurants.csv into the table ch01.restaurants_staging       


## pgsql2shp: Import shapefiles into a table (command line)
You need it if you want to import Shapefiles into Postgres           
http://www.bostongis.com/pgsql2shp_shp2pgsql_quickguide.bqg  

```
shp2pgsql -s 4269:2163 -g geom
 -I /data/roadtrl020.shp ch01.highways_staging
| psql -h localhost -U postgres -p 5432 -d postgis_in_action    
```
PS: You don't need to create the table ch01.highways_staging beforehand. It will be created while importing the shapefile.


## PostGIS SQL      
### https://postgis.net/docs/reference.html       

### Spatial Relationships and Measurements     
https://postgis.net/docs/reference.html#Spatial_Relationships_Measurements      
E.g. ST_DWithin or ST_Intersects                       
              
One thing that confuses new spatial database users is the parallels between the two terminologies. In general SQL lingua franca, you have UNION, INTERSECT, and EXCEPT, which talk about table rows. When you add space to the mix, you have parallel terminology for geometries: ST_Union (which is like a UNION), ST_Collect (which is like a UNION ALL), ST_Intersection (which is like INTERSECT), and ST_Difference (which is like EXCEPT) serve the same purposes for geometries.      


## ORM for Node
http://docs.sequelizejs.com              
Sequelize is a promise-based ORM for Node.js v4 and up. It supports the dialects PostgreSQL, MySQL, SQLite and MSSQL and features solid transaction support, relations, read replication and more.      


## Books, Guides

### PostGIS in Action
Book: https://www.manning.com/books/postgis-in-action-second-edition          
Code is available here: http://www.postgis.us/chapters_edition_2       

### Guide
http://postgresguide.com

### Troubleshooting       
https://confluence.atlassian.com/kb/postgres-troubleshooting-and-how-to-guides-779164471.html



## Other

### DB Samples to play around         
https://www.postgresql.org/ftp/projects/pgFoundry/dbsamples/              
E.g. comes for example with ISO 3166-1 from 2006                  

### OpenGeoSuite 
http://katiekowalsky.me/posts/2015/03/25/shapefile-importer.html            
https://connect.boundlessgeo.com/docs/suite/4.8/intro/whatis.html              


