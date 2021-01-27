---
title: "MySQL"
date: 2019-10-14T09:41:30+08:00
---

mysql.cs.nott.ac.uk

### ER Diagram
1 single fruit can only be sold to one destination
1 student can enrolled in many lectures

### LAB 1
```sql
select round(countries_gdp / countries_population,2) as 'gdp_per_capita', countries_name
from Countries
order by gdp_per_capita DESC;

select countries_name from Countries
where countries_area > 20000;

select countries_name from Countries
where countries_area between 100000 and 200000;

select countries_name from Countries
where countries_name like 'M%';

# wildcard '% _'

select sum(countries_population) as 'total'
from Countries;
```

### Create table
```sql
use zy19165;
CREATE TABLE Artist ( 
	artist_id INT NOT NULL AUTO_INCREMENT,  
    artist_name VARCHAR(255) NOT NULL, 
    CONSTRAINT pk_artist PRIMARY KEY (artist_id),  
    CONSTRAINT ck_artist UNIQUE (artist_name)  
);
  
 CREATE TABLE Album ( 
	album_id INT NOT NULL AUTO_INCREMENT, 
    artist_id INT NOT NULL, 
    album_title VARCHAR(255) NOT NULL, 
    album_price REAL NOT NULL, 
    album_genre VARCHAR(255) NULL, 
    album_num_tracks INT NULL, 
    CONSTRAINT pk_album PRIMARY KEY (album_id), 
    CONSTRAINT fk_album_artist FOREIGN KEY (artist_id)  
    REFERENCES Artist (artist_id)  
); 
```

### Insert Into
```sql
INSERT INTO Artist (artist_name) 
VALUES ('Muse'), ('Mr.Scruff'), ('Deadmau5'), ('Mark Ronson'), ('Animal Collective');  

INSERT INTO Album (artist_id, album_title, album_price, album_genre)
VALUES (1, 'The Resistance', 11.99, 'Rock'), 
(2, 'Ninjia Tuna', 9.99, 'Electronica'), 
(3, 'For Lack of a Better Name', 9.99, 'Electro House'),
(4, 'Record Collection', 11.99, 'Alternative Rock'),
(4, 'Version', 12.99, 'Pop'),
(5, 'Merriweather Post Pavilion', 12.99, 'Electronica');

INSERT INTO Artist (artist_name)
VALUES ('Mark Ronson & The Business Intl');
```

### Update
```sql
UPDATE Artist SET artist_name = 'Scruff'
	WHERE artist_name = 'Mr.Scruff';
    
UPDATE Album SET artist_id = 6 WHERE album_title = 'Record Collection';
```

### Delete
```sql
DELETE FROM Album Where artist_id = 5;
DELETE FROM Artist WHERE artist_name = 'Animal Collective';  
```

### Useful commands
```sql
ALTER TABLE tableName AUTO_INCREMENT = 1; 
 
# This reset the current value to 1, useful when deleted all rows and want generated starting at 1, or some other specific number.

use ’#’ to comment in sql
```

```sql
SHOW_TABLES;
DESCRIBE tableName;
ALTER TABLE tableName DROP AUTO_INCREMENT
```

## LAB 6
### SELECT, AS, DISTINCT
```sql
select * from Artist;

SELECT albumTitle AS Title, albumGenre AS Genre FROM Album;

SELECT DISTINCT albumGenre AS Genre FROM Album;
```

### WHERE, AND, NOT
```sql
SELECT * FROM Album WHERE albumPrice > 10.00;
SELECT * FROM Album WHERE albumGenre = 'Rock';
SELECT * FROM Album WHERE albumGenre <> 'Rock'; 

SELECT * FROM Album WHERE (albumPrice < 10.00) AND NOT
(albumGenre ='Rock'); 

SELECT * FROM Album WHERE albumGenre = 'pop' and albumPrice > 10.00;

SELECT albumTitle FROM Album WHERE 10.00< albumPrice < 12.00;

SELECT albumTitle FROM Album WHERE albumPrice < 10.00 AND NOT albumGenre = 'Rock' or 'Electronica';
```

### Subqueries, IN, ALL, ANY, EXISTS, LIKE
```sql
SELECT * FROM Album WHERE albumGenre  = 
(
SELECT albumGenre FROM Album WHERE albumTitle = 'The Resistance'
);

SELECT * FROM Album WHERE albumPrice IN 
(
SELECT albumPrice FROM Album WHERE albumGenre = 'Pop'
);

SELECT * FROM Album WHERE albumPrice > ANY
(
SELECT albumPrice FROM Album
);

Select artistID FROM Album WHERE Album.albumGenre = 'Electronica';

SELECT artName FROM Artist WHERE artID in 
(
select artistID FROM Album WHERE albumPrice < 10.00
);

-- Find names of albums produced by those Artists who have a single word as their name
select artName FROM Artist WHERE artName NOT like '% %';

SELECT * FROM Album WHERE albumGenre = 'Pop' or albumPrice > 10;

SELECT * FROM Album WHERE albumPrice BETWEEN 10 and 12;

SELECT * FROM Album WHERE albumPrice < 10 and (albumGenre = 'Rock' or albumGenre = 'Electronica');

SELECT * FROM Artist, Album WHERE Artist.artID = Album.artistID and artName = 'Muse';

SELECT * FROM Album WHERE albumPrice > ANY
(SELECT albumPrice FROM Album);

SELECT artID FROM Artist WHERE EXISTS
(SELECT artistID FROM Album WHERE Artist.artID = Album.artistID and albumGenre = 'Electronica');

SELECT artName FROM Artist WHERE artID IN
(SELECT artID FROM Album WHERE albumPrice < 10);
```

### GROUP BY, ORDER BY, HAVING
```sql
SELECT Artist.artName, COUNT(Album.albumTitle) AS Count, AVG(Album.albumPrice) FROM Album, Artist
WHERE Artist.artID = Album.artistID AND NOT Album.albumGenre = 'Electronica'
GROUP BY artName
HAVING Count > 1; 
```

### COUNT, SUM, AVG, MAX, MIN
```sql
COUNT(DISTINCT column)

# use group by
SELECT artistID, min(albumPrice) FROM Album GROUP BY artistID;

SELECT albumPrice, count(*) FROM Album WHERE albumPrice = 11.99;

SELECT albumGenre, COUNT(*) AS Count FROM Album GROUP BY albumGenre HAVING Count > 0;

SELECT artName, COUNT(albumTitle) as Count FROM Artist, Album WHERE Artist.artID = Album.artistID GROUP BY
artName HAVING Count > 0;

SELECT artName, AVG(albumPrice) as avgPrice, COUNT(albumTitle) as albumNum FROM Artist, Album WHERE 
Artist.artID = Album.artistID and albumGenre != 'Electornica'
GROUP BY artNAME;
```

