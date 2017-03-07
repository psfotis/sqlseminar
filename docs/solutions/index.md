---
layout: template1
comments: false
---

<div class="jumbotron" style="background-color: #F4FDFD">
<p style="font-size: 11pt; text-align: justify">
Lets start querying the flightsdb. In this page you will find many questions that you need
to translate to SQL queries and run against the flightsdb database. The questions are categorized into 6 sections:
</p>

<ol>
<li><a href="#s1">GET FAMILIAR WITH THE CONTENTS OF THE DATABASE</a></li>
<li><a href="#s2">INTERESTING FACTS USING SIMPLE <code class="highlighter-rouge">SELECT-FROM-WHERE</code> QUERIES</a></li>
<li><a href="#s3">USE <code class="highlighter-rouge">JOINS</code> FOR POWERFUL INSIGHTS</a></li>
<li><a href="#s4">USE SQL FOR MEANINGFUL ANALYSIS</a></li>
<li><a href="#s5">GRAPH ANALYTICS USING SQL</a></li>
<li><a href="#s6">COMPLICATED MATHEMATICAL CALCULATIONS USING SQL</a></li>
</ol>

<p style="font-size: 11pt; text-align: justify">
The questions vary in complexity. Make sure to answer the following questions: <b>Q1-Q9, Q13-Q16, Q21-Q24</b>. The rest of the questions require some familiarity with SQL. So, target them if you finish the questions above. 
<br/><br/>
<span style="color:red; font-weight:bolder;">Note</span>: You do not need to finish all questions during the hands-on session. Whichever questions you do not reply, try to answer them at your own convenience. Let the instructor know for any of your questions over email.
</p>
</div>

For your convenience, following is the schema of the flightsdb database with explanation of the columns:

```
TABLE airlines (
   alid integer PRIMARY KEY,      -- Id of the airline
   name text,                     -- Name of the airline
   iata varchar(2),               -- 2-letter IATA code. empty or null if not assigned/unknown 
   icao varchar(3),               -- 3-letter ICAO code. empty or null if not assigned
   callsign text,                 -- Airline callsign
   country text,                  -- Country or territory where airline is incorporated
   active varchar(2)              -- "Y" if the airline is or has until recently been operational,
                                  -- "N" if it is defunct.
);


TABLE airports (
   apid integer PRIMARY KEY,      -- Id of the airport
   name text NOT NULL,            -- Name of airport
   city text,                     -- Main city served by airport
   country text,                  -- Country or territory where airport is located
   x double precision,            -- Latitude of airport: Decimal degrees, usually to six
                                  -- significant digits. Negative is South, positive is North
   y double precision,            -- Longitude of airport: Decimal degrees, usually to six 
                                  -- significant digits. Negative is West, positive is East
   elevation bigint,              -- Altitude of airport measured in feets
   iata character varchar(3),     -- 3-letter IATA code. empty or null if not assigned/unknown
   icao character varchar(4)      -- 4-letter ICAO code. empty or null if not assigned
   
);

TABLE routes (
   rid integer PRIMARY KEY,
   dst_apid integer,              -- Id of destination airport
   dst_ap varchar(4),             -- 3-letter (IATA) or 4-letter (ICAO) code of the destination airport
   src_apid bigint,               -- Id of source airport
   src_ap varchar(4),             -- 3-letter (IATA) or 4-letter (ICAO) code of the source airport
   alid bigint,                   -- 2-letter (IATA) or 3-letter (ICAO) code of the airline
   airline varchar(4),            -- 2-letter (IATA) or 3-letter (ICAO) code of the airline
   codeshare text                 -- "Y" if this flight is a codeshare (that is, not operated by 
                                  -- Airline, but another carrier), empty otherwise
);
```

### <a name="s1"></a>1. GET FAMILIAR WITH THE CONTENTS OF THE DATABASE

**Q1. To get familiar with the contents of the database, use the ``SELECT * FROM TABLE`` query construct for each table in the database (i.e., airlines, airports, routes)**

**A1.**

```
SELECT * FROM airports;

SELECT * FROM airlines;

SELECT * FROM routes;
```

**Q2. Most likely, from the results of Q1 you got overwhelmed from the multiple attributes in each table.  Use the ``SELECT column1, column2, .., columnN FROM TABLE`` construct to limit the attributes that you see in the result. In particular, write SQL queries:**

a. to project only the name, city, and country of each airport.

b. to project only the name of an airline, the country where the airline is incorporated, and if it is still active.

c. to project only the names and ids of the source and destination airports along with the number of stops in  the routes table.

**A2.**

```
-- A2.a.
SELECT name, city, country
FROM   airports;


-- A2.b.
SELECT name, country, active
FROM   airlines;

-- A2.c.

SELECT src_apid, src_ap, dst_apid, dst_ap, stops
FROM   routes; 
```


### <a name="s2"></a>2. INTERESTING FACTS USING SIMPLE ```SELECT-FROM-WHERE``` QUERIES

**Q3. Find the name, city, country, and altitude (or elevation) of the JFK airport.** Hint: We need to find the record with iata='JFK' in the airports table and project only the name, city, country, and elevation attributes.

**A3.**

```
SELECT name, city, country, elevation
FROM   airports
WHERE  iata = 'JFK';
```

**Q4. Find the routes operated by American Airlines that are codeshared.** Hint: we need the records in the routes table with airline='AA' and codeshare='Y'.

**A4.**

```
SELECT * 
FROM   routes 
WHERE  airline = 'AA' AND codeshare='Y';
```

**Q5. Find the airports located in Cuba or Argentina.**

**A5.**

```
SELECT * 
FROM   airports 
WHERE  country = 'Cuba' OR country = 'Argentina';
```

**Q6. Find the airlines whose name starts with 'Orbit'.** Hint: you need to use the ``LIKE`` operator on the name attribute of the airports table.

**A6.**

```
SELECT *
FROM   airlines
WHERE  name LIKE 'Orbit%'
```

**Q7. Find the airports within a bounding box.** For this exercise you will need to find the airports with latitude (i.e., attribute ```x``` in the airports table) and longitude (i.e., attribute ``y`` in the airports table) within the bounding box ```[-74.2589, 40.4774, -73.7004, 40.9176]```. What this means is that we need ``y`` to be within ``[-74.2589, -73.7004]`` and ``x`` within ``[40.4774, 40.9176]``.

**A7.**

```
SELECT * 
FROM   airports 
WHERE  -74.2589 < x AND x < -73.7004 AND
       40.4774  < y AND y < 40.9176;
```


### <a name="s3"></a>3. USE ```JOINS``` FOR POWERFUL INSIGHTS

**Q8. Find the routes with destination airport in Italy.**

**A8.**

```
SELECT *
FROM   routes, airports 
WHERE  routes.dst_apid = airports.apid AND
       country = 'Italy';
```

**Q9. Extend the query of Q9 to return routes with destination airport in Italy operated by the airline with name 'American Airlines'.**

**A9.**

```
SELECT *
FROM  routes, airports, airlines
WHERE routes.dst_apid = airports.apid AND
      airports.country = 'Italy' AND
      routes.alid=airlines.alid AND
      airlines.name='American Airlines';
```

**Q10. Find the domestic routes.** A domestic route needs to have a source and destination airport in the same country. For each route project the names of the source and destination airports. Hint: You will need to use two different instances of the airports table. One to join with the routes.src_apid and another one to join with routes.dst_apid.

More specifically, your SQL query will have the following structure:

```
SELECT ...
FROM   routes, airports AS A1, airports AS A2
WHERE  ...
```

Make sure that you understand why we need two instances of the airports table.

**A10.**

```
SELECT  A1.name AS source_airport,
        A2.name as destination_airport
FROM    routes, 
        airports AS A1,
        airports AS A2
WHERE   A1.apid = src_apid AND             
        A2.apid = dst_apid AND             
        A1.country = A2.country;          -- domestic route
```

**Q11. Similarly to Q6, find the routes that are international.** International routes should not have source and destination airports in the same country. Furthermore, we only need the international routes operated by 'American Airlines'. Make sure to avoid the routes with empty airlines.

**A11.**

```
SELECT  airlines.name AS airline, 
        A1.name AS source_airport,
        A2.name as destination_airport
FROM    routes, 
        airlines, 
        airports AS A1,
        airports AS A2
WHERE   routes.airline = airlines.iata AND
        routes.airline != '' AND            -- avoid empty airlines
        airlines.iata='AA' AND
        A1.apid = src_apid AND             
        A2.apid = dst_apid AND             
        A1.country != A2.country;           -- international route
```

**Q12. Find the routes from the United States to Canada operated by American Airlines.**

**A12.**

```
SELECT  airlines.name AS airline, 
        A1.name AS source_airport,
        A2.name as destination_airport
FROM    routes, 
        airlines, 
        airports AS A1,
        airports AS A2
WHERE   routes.airline = airlines.iata AND  
        routes.airline != '' AND
        airlines.iata='AA' AND
        A1.apid = src_apid AND
        A2.apid = dst_apid AND
        A1.country = 'Canada' AND
        A2.country = 'United States'
```


### <a name="s4"></a>4. USE SQL FOR MEANINGFUL ANALYSIS

**Q13. How many airports are there per country? Order the countries by decreasing number of airports.**

**A13.**
```
SELECT   COUNT(*), country
FROM     airports
GROUP BY country
ORDER BY COUNT(*) DESC;
```

**Q14. How many airports are there per city in the United States? Order the cities by decreasing number of airports and project only the cities. Use the ``HAVING`` clause to return only the cities with more than 3 airports.**

**A14.**
```
SELECT   COUNT(*), city 
FROM     airports
WHERE    country = 'United States'
GROUP BY city
HAVING   COUNT(*) > 3
ORDER BY COUNT(*) DESC;
```


**Q15. Find the number of routes for each source airport. Project only the number of routes and corresponding id of the airport (i.e., src_apid). Order the results by decreasing number of routes.**

**A15.**
```
SELECT COUNT(*), src_apid
FROM routes 
GROUP BY src_apid
ORDER BY count(*) desc;
```

**Q16. Find the average,  max, min, and standard deviation of routes operated by American Airlines across cities in the United States. You need to account only for routes with source airport located in a city in the United States.** Hint: First, write a SQL query to find the number of routes per city in the United States operated by American Airlines. Again, account only for routes with source airport located in a city in the United States. Then, use this query as a nested subquery to calculate the average, max, min, and standard deviation.

**A16.**

```
SELECT AVG(cnt), MAX(cnt), MIN(cnt), STDDEV(cnt) 
FROM ( 
    SELECT   COUNT(*) as cnt, city
    FROM     routes, airports
    WHERE    routes.src_apid = airports.apid AND
             airline = 'AA' AND
             country = 'United States'
    GROUP BY city
) AS S;
```


**Q17. Find the number of routes that have as destination the JFK airport.**

**A17.**

```
SELECT   COUNT(*), dst_apid, airports.name 
FROM     routes, airports 
WHERE    dst_apid = airports.apid AND
         airports.iata = 'JFK'
GROUP BY dst_apid, airports.name;
```

**Q18. How many routes are domestic and how many routes are international?**

**A18.**
```
-- domestic
SELECT COUNT(*) AS domestic_routes FROM (
    SELECT  A1.name AS source_airport,
            A2.name as destination_airport
    FROM    routes,
            airports AS A1,
            airports AS A2
    WHERE   A1.apid = src_apid AND
            A2.apid = dst_apid AND
            A1.country = A2.country;
) AS S

-- international 
SELECT COUNT(*) AS international_routes FROM (
    SELECT  A1.name AS source_airport,
            A2.name as destination_airport
    FROM    routes,
            airports AS A1,
            airports AS A2
    WHERE   A1.apid = src_apid AND
            A2.apid = dst_apid AND
            A1.country != A2.country;
) AS S
```

**Q19. Find the source airports with amount of routes above the average number of routes for all source airports.**

**A19.**

```
SELECT   COUNT(*), airports.name                      
FROM     airports, routes
WHERE    airports.apid = routes.src_apid
GROUP BY airports.apid
HAVING COUNT(*) > (
         SELECT AVG(cnt) 
         FROM (
             SELECT COUNT(*) AS cnt
             FROM routes, airports
             WHERE routes.src_apid = airports.apid
             GROUP BY apid
         )AS S
       );
```

**Q20. Find the route with the most airlines. Assume there is only one route with the most airlines. Then, write another SQL query to find the names of the airlines in this route.**

**A20.**
```
SELECT   COUNT(DISTINCT airlines.name) AS cnt, src_apid, dst_apid 
FROM     routes, airlines
WHERE    airlines.alid = routes.alid
GROUP BY src_apid, dst_apid
ORDER BY cnt DESC
LIMIT 1;
```

```
SELECT DISTINCT airlines.name
FROM (
       SELECT   COUNT(DISTINCT airlines.name) AS cnt, src_apid, dst_apid 
       FROM     routes, airlines
       WHERE    airlines.alid = routes.alid
       GROUP BY src_apid, dst_apid
       ORDER BY cnt DESC
       LIMIT 1
     ) AS most_airlines_route, routes, airlines
WHERE routes.src_apid = most_airlines_route.src_apid AND
      routes.dst_apid = most_airlines_route.dst_apid AND
      airlines.alid = routes.alid
```




### <a name="s5"></a>5. GRAPH ANALYTICS USING SQL

Each route has a destination and a source airport. Conceptually, we can model the routes as a directed graph, where nodes correspond to individuals airports and directed edges denote routes between nodes. Similarly, we can also conceptually model routes between cities or countries as a directed graphs. Each city is a node that corresponds to all airports in that city. Similarly, each country corresponds to a node that corresponds to all airports in that country.

**Q21. Write 2 SQL queries that calculate the in- and out-degree for each airport, respectively.** The in-degree of an airport is the number of times that it was a destination in a route. Similarly, the out-degree is the number of times it was a source in a route. For each query, return the in- or out-degree, the id of the source or destination airport, and the name of the airport. Finally, order the results by the in- or out-degree.

**A21.**
```
-- out-degree
SELECT   COUNT(*) AS out_degree, src_apid, airports.name 
FROM     routes, airports 
WHERE    routes.src_apid = airports.apid 
GROUP BY src_apid, airports.name
ORDER BY out_degree DESC;

-- in-degree
SELECT   COUNT(*) AS in_degree, dst_apid, airports.name 
FROM     routes, airports 
WHERE    routes.dst_apid = airports.apid 
GROUP BY dst_apid, airports.name
ORDER BY in_degree DESC;
```

**Q22. Similarly to Q21, calculate the in- and out-degree for each city.**

**A22.**
```
-- out-degree
SELECT count(*) AS out_degree, airports.city 
FROM routes, airports 
WHERE routes.src_apid = airports.apid 
GROUP BY airports.city
ORDER BY out_degree desc;

-- in-degree
SELECT count(*) AS in_degree, airports.city 
FROM routes, airports 
WHERE routes.dst_apid = airports.apid 
GROUP BY airports.city
ORDER BY in_degree desc;
```

**Q23. Calculate the in- and out-degree for each country.**

**A23.**
```
-- out-degree
SELECT count(*) AS out_degree, airports.country 
FROM routes, airports 
WHERE routes.src_apid = airports.apid 
GROUP BY airports.country
ORDER BY out_degree desc;

-- in-degree
SELECT count(*) AS in_degree, airports.country 
FROM routes, airports 
WHERE routes.dst_apid = airports.apid 
GROUP BY airports.country
ORDER BY in_degree desc;
```

**Q24. Which city is the most prestigious destination?** Hint: we need to find the city with the highest in-degree. Assume there is only one city with the highest in-degree.

**A24.**
```
SELECT count(*) AS in_degree, airports.city 
FROM routes, airports 
WHERE routes.dst_apid = airports.apid 
GROUP BY airports.city
ORDER BY in_degree desc
LIMIT 1;
```

**Q25. Which countries are the least prestigious destinations?** As oppose to Q4, where there was only one city with the highest in-degree, now there are ties (i.e., multiple countries with the same least amount of in-degree.). To construct this query, you need to use the ```HAVING``` clause. We need to return all countries who *have* in-degree equal to the minimum in-degree among all countries.

```
SELECT   COUNT(*) AS in_degree, airports.country
FROM     routes, airports 
WHERE    routes.dst_apid = airports.apid 
GROUP BY airports.country 
HAVING   COUNT(*) = (
                      SELECT MIN(in_degree)
                      FROM (
                            SELECT  COUNT(*) AS in_degree, airports.country 
                            FROM    routes, airports
                            WHERE   routes.dst_apid = airports.apid
                            GROUP BY airports.country
                      ) AS in_degrees
                    );
```

**Q26. Find the airports that are 2 hops away.** This means there is no direct route from airport A to C but there is at least a route from A → B and another route from B → C. So, to get from A to B we need to go through C. In the routes table there is an attribute called stops indicating that this route has an intermediate stop. For this exercise, assume there is no intermediate stop and every route in the routes table is direct.

**Q27. Find the airports that are 3 hops away.** Similarly to Q6, but now we want the airports A and D for which there are airports B and C such that there are routes A → B, B → C, and C → D. Of course there are no direct or 2-hops routes from A to D.  

**Q28. Can we write a general purpose SQL query to find the airports that are N hops away, based on what we discussed in class and the way you answered Q6 and Q7?**

* **Hint 1**: No, we can't. In general, we can construct a query if someone gives a number for N. But we can't construct a SQL query for any N. Can you tell why?
* **Hint 2**: To support such kind of queries, SQL has introduced constructs for recursive computation (i.e., recursive functions and WITH RECURSIVE constructs) that we did not cover in class. With the introduction of recursive constructs in SQL we can compute anything we may want. Why would we need a recursive computation in this example?

### <a name="s6"></a>6. COMPLICATED MATHEMATICAL CALCULATIONS USING SQL

**Q29.** For this exercise you will need to construct a SQL query that calculates the Haversine distance from New York City to every airport in the world**.** Note the following:

* The formula for the Haversine distance of two points in the globe with coordinates (lat1, lng2) and (lat2, lng2) is:  
<img src="/sqlseminar/assets/img/haversine.png" style="width: 750px;"/>

* Assume the (latitude, longitude) of New York City to be: (40.748817, -73.985428)
* PostgresSQL has built-in the following functions that will be useful for the calculation of the this formula
    * RADIANS(x): converts a number x to radians, 
    * COS(x): computes the cosine of x, 
    * SIN(x): computes the sine of x, 
    * SQRT(x): computes the square root of x,
    * ATAN2(x, y): inverse tangent of x/y
* Also, PostgresSQL supports **+**, **\***, and **^** for addition, multiplication, and power, respectively.
* To construct the query that calculates the haversine distance, use the x (i.e., latitude) and y (i.e., longitude) coordinates of the airports and the (latitude, longitude) of New York City. 
* To calculate the squares of the sines of the differences of latitudes and longitudes as calculated in the distance above, we can do the following:

```
SELECT sin(radians(y-40.748817)/2.0)^2.0, sin(radians(x+73.985428)/2.0)^2.0
FROM airports
```

Continue the construction of the SQL query for the Haversine distance. Along with the distance, project the name, country, and city of the airport. Hint: Instead of recalculating the **a** parameter twice for the calculation of parameter **c** use a nested subquery.

**A29.**

```
SELECT 2 * atan2(A.a, sqrt(1-A.a)) * 6371000.0 as d,
       name, country, city 
FROM (
    SELECT sin(radians(y-40.748817)/2.0)^2.0 + 
                cos(radians(y))*
                cos(radians(40.748817))*
                sin(radians(x+73.985428)/2.0)^2.0 AS a, name, country, city
    FROM airports) AS A 
ORDER BY d asc;
```
