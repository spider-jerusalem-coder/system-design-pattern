# Postgresql Geo

### 1. What extension to use
postgis

### 2. How to create table
```
create table place {
  place_id: str
  location GEOGRAPHY(POINT, 4326)
}
```
> Also create index on location

### 3. How to search for a location which 10000 metres available from a given ?
```
SELECT * FROM places
WHERE ST_DWithin(
    location, 
    ST_MakePoint(longitude, latitude)::geography,
    10000 # metres
);
```
