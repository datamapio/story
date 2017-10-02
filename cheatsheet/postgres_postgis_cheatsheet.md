# PostgreSQL (OSX specific)

## Postgres and PostGIS = Postgres.app
https://postgresapp.com/documentation/

## GUI Client Apps
https://www.pgadmin.org/
https://eggerapps.at/postico/
http://www.macpostgresclient.com/SQLProPostgres

## psql (Command Line): Import csv into a table
Can be started from Postgres.app
Important if you want to copy a csv file into your table

```
copy ch01.restaurants_staging
 FROM '/data/restaurants.csv' DELIMITER as ',';
```


## pgsql2shp (Command Line): Import shapefiles into a table
You need it if you want to import Shapefiles into Postgres           
http://www.bostongis.com/pgsql2shp_shp2pgsql_quickguide.bqg        

## OpenGeoSuite
http://katiekowalsky.me/posts/2015/03/25/shapefile-importer.html


## PostGIS in Action
Book: https://www.manning.com/books/postgis-in-action-second-edition          
Code is available here: http://www.postgis.us/chapters_edition_2       

## Guide
http://postgresguide.com/


