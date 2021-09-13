## SQLZOO_Solutions

## Sections:
1. [SELECT basics](#select-basics)
2. [SELECT from WORLD](#select-from-world)
3. [SELECT from NOBEL](#select-from-nobel)
4. [SELECT within SELECT Tutorial](#select-within-select-tutorial)
5. [SUM and COUNT](#sum-and-count)
6. [JOIN](#join)
7. [More JOIN](#more-join)
8. [Using NULL](#using-null)
9. [Self JOIN](#self-join)

## SELECT basics
### SELECT basics [(ENGLISH 1-3)](https://sqlzoo.net/wiki/SELECT_basics)
1. The example uses a WHERE clause to show the population of 'France'. Note that strings (pieces of text that are data) should be in 'single quotes';
* Modify it to show the population of Germany
```sql
  SELECT population
  FROM world
  WHERE name = 'Germany';
```

2. Checking a list: The word IN allows us to check if an item is in a list. The example shows the name and population for the countries 'Brazil', 'Russia', 'India' and 'China'.
* Show the name and the population for 'Sweden', 'Norway' and 'Denmark'.
```sql
  SELECT name, population
  FROM world
  WHERE name IN ('Sweden', 'Norway', 'Denmark');
```
3. Which countries are not too small and not too big? BETWEEN allows range checking (range specified is inclusive of boundary values). The example below shows countries with an area of 250,000-300,000 sq. km.
* Modify it to show the country and the area for countries with an area between 200,000 and 250,000.
```sql
  SELECT name, area 
  FROM world
  WHERE area BETWEEN 200000 AND 250000;
```
### SELECT basics [(CHINESE 2)](https://sqlzoo.net/wiki/SELECT_basics/zh)
2. 查詢顯示面積為 5,000,000 以上平方公里的國家,該國家的人口密度(population/area)。人口密度並不是 WORLD 表格中的欄,但我們可用公式(population/area)計算出來。
* 修改此例子,查詢面積為 5,000,000 以上平方公里的國家,對每個國家顯示她的名字和人均國內生產總值(gdp/population)。
```sql
  SELECT name, gdp/population
  FROM world
  WHERE area > 5000000;
```
## SELECT from WORLD
### SELECT from WORLD [(ENGLISH 1-13)](https://sqlzoo.net/wiki/SELECT_from_WORLD_Tutorial)
1. Read the notes about this table. Observe the result of running this SQL command to show the name, continent and population of all countries.
```sql
  SELECT name, continent, population
  FROM world;
```
2. How to use WHERE to filter records. Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.
```sql
  SELECT name
  FROM world
  WHERE population > 200000000;
```
3. Give the name and the per capita GDP for those countries with a population of at least 200 million. HELP:How to calculate per capita GDP
```sql
  SELECT name, GDP/population
  FROM world
  WHERE population > 200000000;
```
4. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
```sql
  SELECT name, population/1000000
  FROM world
  WHERE continent = 'South America';
```
5. Show the name and population for France, Germany, Italy
```sql
  SELECT name, population
  FROM world
  WHERE name IN ('France','Germany','Italy');
```
6. Show the countries which have a name that includes the word 'United'
```sql
  SELECT name
  FROM world
  WHERE name LIKE '%united%';
```
7. Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million.
* Show the countries that are big by area or big by population. Show name, population and area.
```sql
  SELECT name, population, area
  FROM world
  WHERE area > 3000000 or population > 250000000;
```
8. Exclusive OR (XOR). Show the countries that are big by area (more than 3 million) or big by population (more than 250 million) but not both. Show name, population and area.
* Australia has a big area but a small population, it should be included.
* Indonesia has a big population but a small area, it should be included.
* China has a big population and big area, it should be excluded.
* United Kingdom has a small population and a small area, it should be excluded.
```sql
  SELECT name, population, area
  FROM world
  WHERE (area > 3000000 AND population < 250000000)
  or (area < 3000000 and population > 250000000);
```
9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Use the ROUND function to show the values to two decimal places.
* For South America show population in millions and GDP in billions both to 2 decimal places.(Millions and billions)
```sql
  SELECT name, ROUND(population/1000000,2), ROUND(gdp/1000000000,2)
  FROM world
  WHERE continent = 'South America';
```
10. Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000.
* Show per-capita GDP for the trillion dollar countries to the nearest $1000.
```sql
  SELECT name, ROUND(gdp/population, -3)
  FROM world
  WHERE gdp > 1000000000000;
```
11. Greece has capital Athens. Each of the strings 'Greece', and 'Athens' has 6 characters.Show the name and capital where the name and the capital have the same number of characters.
* You can use the LENGTH function to find the number of characters in a string
```sql
  SELECT name, capital 
  FROM world 
  WHERE LEN(name) = LEN(capital);
```
12.
The capital of Sweden is Stockholm. Both words start with the letter 'S'.
* Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.
* You can use the function LEFT to isolate the first character.
* You can use <> as the NOT EQUALS operator.
```sql
  SELECT name, capital 
  FROM world 
  WHERE　LEFT(name, 1) = LEFT(capital, 1) AND name <> capital;
```
13.　Equatorial Guinea and Dominican Republic have all of the vowels (a e i o u) in the name. They don't count because they have more than one word in the name.
* Find the country that has all the vowels and no spaces in its name.
* You can use the phrase name NOT LIKE '%a%' to exclude characters from your results.
* The query shown misses countries like Bahamas and Belarus because they contain at least one 'a'
```sql
  SELECT name
  FROM world
  WHERE name LIKE '%a%'
  AND name LIKE '%e%'
  AND name LIKE '%i%'
  AND name LIKE '%o%'
  AND name LIKE '%u%'
  AND name NOT LIKE '% %';
```
### SELECT from WORLD [(CHINESE 11-13)](https://sqlzoo.net/wiki/SQLZOO:SELECT_from_WORLD_Tutorial/zh)
11. The CASE statement shown is used to substitute North America for Caribbean in the third column.
* Show the name - but substitute Australasia for Oceania - for countries beginning with N.
```sql
  SELECT name, 
       CASE WHEN continent='Oceania' THEN 'Australasia' ELSE continent END
  FROM world
  WHERE name LIKE 'N%';
```
12. Show the name and the continent - but substitute Eurasia for Europe and Asia; substitute America - for each country in North America or South America or Caribbean. Show countries beginning with A or B
```sql
  SELECT name,
         CASE WHEN continent='Europe' or continent='Asia' THEN 'Eurasia'
              WHEN continent in ('North America','South America','Caribbean') THEN 'America' ELSE continent END
  FROM world
  WHERE name LIKE 'A%' or name LIKE 'B%';
```
13. Put the continents right...
* Oceania becomes Australasia
* Countries in Eurasia and Turkey go to Europe/Asia
* Caribbean islands starting with 'B' go to North America, other Caribbean islands go to South America
* Show the name, the original continent and the new continent of all countries.
```sql
  SELECT name, continent,
         CASE WHEN continent = 'Oceania' THEN 'Australasia'
              WHEN continent = 'Eurasia' OR name = 'Turkey' THEN 'Europe/Asia'
              WHEN continent = 'Caribbean' AND name LIKE 'b%' THEN 'North America'
              WHEN continent = 'Caribbean' AND name NOT LIKE 'b%' THEN 'South America' ELSE continent END
  FROM world
  ORDER BY name;
```
## SELECT from NOBEL
### SELECT from NOBEL [(ENGLISH 1-13)](https://sqlzoo.net/wiki/SELECT_from_Nobel_Tutorial)
1. Change the query shown so that it displays Nobel prizes for 1950.
```sql
  SELECT yr, subject, winner
  FROM nobel
  WHERE yr = 1950;
```
2. Show who won the 1962 prize for Literature.
```sql
  SELECT winner
  FROM nobel
  WHERE yr = 1962 AND subject = 'Literature';
```
3. Show the year and subject that won 'Albert Einstein' his prize.
```sql
  SELECT yr, subject
  FROM nobel
  WHERE winner = 'Albert Einstein';
```
4. Give the name of the 'Peace' winners since the year 2000, including 2000.
```sql
  SELECT winner
  FROM nobel
  WHERE subject = 'Peace' AND yr >= 2000;
```
5. Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.
```sql
  SELECT yr, subject, winner
  FROM nobel
  WHERE subject = 'Literature' AND yr BETWEEN 1980 AND 1989;
```
6. Show all details of the presidential winners:
* Theodore Roosevelt
* Woodrow Wilson
* Jimmy Carter
* Barack Obama
```sql
  SELECT * FROM nobel
  WHERE winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter', 'Barack Obama');
```
7. Show the winners with first name John
```sql
  SELECT winner
  FROM nobel
  WHERE winner LIKE 'JOHN %';
```
8. Show the year, subject, and name of Physics winners for 1980 together with the Chemistry winners for 1984.
```sql
  SELECT * FROM nobel
  WHERE yr = 1980 AND subject = 'Physics'
  OR yr = 1984 AND subject = 'Chemistry';
```
9. Show the year, subject, and name of winners for 1980 excluding Chemistry and Medicine
```sql
  SELECT * FROM nobel
  WHERE yr = 1980
  AND subject NOT IN ('Chemistry','Medicine');
```
10. Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
```sql
  SELECT * FROM nobel
  WHERE yr < 1910 AND subject = 'Medicine'
  OR yr >= 2004 AND subject = 'Literature';
```
11. Find all details of the prize won by PETER GRÜNBERG (Non-ASCII characters)
```sql
  SELECT *
  FROM nobel
  WHERE winner LIKE 'peter gr%nberg';
```
12. Find all details of the prize won by EUGENE O'NEILL (Escaping single quotes)
```sql
  SELECT *
  FROM nobel
  WHERE winner = 'Eugene O''Neill';
```
13. Knights in order
* List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
```sql
  SELECT winner, yr, subject
  FROM nobel
  WHERE winner LIKE 'sir%'
  ORDER BY yr DESC, winner;
```
14. The expression subject IN ('Chemistry','Physics') can be used as a value - it will be 0 or 1.
* Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.
```sql
  SELECT winner, subject
  FROM nobel
  WHERE yr=1984
  ORDER BY
  CASE WHEN subject IN ('Physics','Chemistry') THEN 1 ELSE 0 END, subject, winner;
```
## SELECT within SELECT Tutorial
### SELECT within SELECT Tutorial [(ENGLISH 1-10)](https://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial)

1. List each country name where the population is larger than 'Russia'.
```sql
  SELECT name FROM world
  WHERE population > (SELECT population FROM world
                      WHERE name='Russia');
```
2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```sql
  SELECT name FROM world
  WHERE continent = 'Europe'
  AND gdp/population > (SELECT gdp/population
                        FROM world
                        WHERE name = 'United Kingdom');
```
3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```sql
  SELECT name, continent FROM world
  WHERE continent IN (SELECT continent FROM world
                      WHERE name IN ('Argentina','Australia'));
  ORDER BY name
```
4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.
```sql
  SELECT name, population FROM world
  WHERE population > (SELECT population FROM world
                      WHERE name = 'Canada')
  AND population < (SELECT population FROM world
                    WHERE name = 'Poland');
```
5. Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.
Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
```sql
  SELECT name, CONCAT(ROUND(population/(SELECT population FROM world
                      WHERE name = 'Germany')*100,0),'%')
   FROM world WHERE continent = 'Europe';
```
6. Which countries have a GDP greater than every country in Europe? (Some countries may have NULL gdp values)

```sql
  SELECT name FROM world
  WHERE gdp > ALL(SELECT gdp FROM world
                  WHERE gdp > 0 AND continent = 'Europe');
```
7. Find the largest country (by area) in each continent, show the continent, the name and the area:

```sql
  SELECT continent, name, area FROM world x
  WHERE area >= ALL
  (SELECT area FROM world y
   WHERE y.continent=x.continent
   AND area>0);
```
8. List each continent and the name of the country that comes first alphabetically.
```sql
  SELECT continent, name
  FROM world x
  WHERE name <= ALL(SELECT name FROM world y WHERE y.continent = x.continent);
```
9. Find the continents where all countries have a population <= 25000000.
Then find the names of the countries associated with these continents. Show name, continent and population.
```sql
  SELECT name, continent, population
  FROM world x
  WHERE 25000000  > ALL(SELECT population FROM world y WHERE x.continent =    y.continent AND y.population > 0);
```
10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.
```sql
  SELECT name, continent
  FROM world x
  WHERE population > ALL(SELECT population*3 FROM world y WHERE x.continent = y.continent AND population > 0 AND y.name != x.name);
```
## SUM and COUNT
### SUM and COUNT [(ENGLISH 1-8)](https://sqlzoo.net/wiki/SUM_and_COUNT)
1. Show the total population of the world.
```sql
SELECT SUM(population)
FROM world;
```
2. List all the continents - just once each.
```sql
SELECT DISTINCT(continent)
FROM world
```
3. Give the total GDP of Africa
```sql
SELECT SUM(gdp) 
FROM world
WHERE continent = 'Africa'
```
4. How many countries have an area of at least 1000000
```sql
SELECT COUNT(name)
FROM world
WHERE area >= 1000000
```
5. What is the total population of ('Estonia', 'Latvia', 'Lithuania')
```sql
SELECT SUM(population)
FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
```
6. For each continent show the continent and number of countries.
```sql
SELECT continent, COUNT(name)
FROM world
GROUP BY continent
```
7. For each continent show the continent and number of countries with populations of at least 10 million.
```sql
SELECT continent, COUNT(name)
FROM world
WHERE population >= 10000000
GROUP BY continent
```
8. List the continents that have a total population of at least 100 million.
```sql
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) > 100000000
```
## JOIN
### JOIN [(ENGLISH 1-11)](https://sqlzoo.net/wiki/The_JOIN_operation)
1.The first example shows the goal scored by 'Bender'.Show matchid and player name for all goals scored by Germany.
```sql
SELECT matchid, player
FROM goal
WHERE teamid = 'GER'
```
2.From the previous query you can see that Lars Bender's goal was scored in game 1012. Notice that the column matchid in the goal table corresponds to the id column in the game table.
Show id, stadium, team1, team2 for game 1012
```sql
SELECT id, stadium, team1, team2
FROM game
WHERE id = 1012
```
3.You can combine the two steps into a single query with a JOIN. You will get all the game details and all the goal details if you use
```sql
SELECT player, teamid, stadium, mdate
FROM game JOIN goal ON (id=matchid)
WHERE teamid = 'GER'
```
4.Use the same JOIN as in the previous question.
Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
```sql
SELECT team1, team2, player
FROM game
JOIN goal ON (id=matchid)
WHERE player LIKE 'Mario%'
```
5.The table eteam gives details of every national team including the coach. You can JOIN goal to eteam using the phrase goal JOIN eteam on teamid=id
Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10
```sql
SELECT player, teamid, coach, gtime
FROM goal
JOIN eteam on (teamid=id)
WHERE gtime<=10
```
6.To JOIN game with eteam you could use either
game JOIN eteam ON (team1=eteam.id) or game JOIN eteam ON (team2=eteam.id)
Notice that because id is a column name in both game and eteam you must specify eteam.id instead of just id
List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
```sql
SELECT mdate,teamname FROM game
JOIN eteam ON (team1 = eteam.id)
WHERE coach = 'Fernando Santos'
```
7.List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
```sql
SELECT player
FROM goal
JOIN game ON (matchid = id)
WHERE stadium = 'National Stadium, Warsaw'
```
8.The example query shows all goals scored in the Germany-Greece quarterfinal.
Instead show the name of all players who scored a goal against Germany.
```sql
SELECT DISTINCT player
FROM game JOIN goal ON matchid = id
WHERE (team1= 'GER' OR team2='GER')
AND teamid != 'GER'
```
9.Show teamname and the total number of goals scored.
```sql
SELECT teamname, COUNT(*)
FROM eteam 
JOIN goal ON id=teamid
GROUP BY teamname
```
10. Show the stadium and the number of goals scored in each stadium.
```sql
SELECT stadium, COUNT(*) 
FROM goal
JOIN game ON (matchid = id)
GROUP BY stadium
```
11.For every match involving 'POL', show the matchid, date and the number of goals scored.
```sql
SELECT matchid, mdate, COUNT(*)
FROM game 
JOIN goal ON matchid = id
WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY matchid, mdate
```
12.For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'
```sql
SELECT matchid, mdate, COUNT(*)
FROM goal
JOIN game ON (matchid=id)
WHERE teamid = 'GER'
GROUP BY matchid, mdate
```
13List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.
```sql
SELECT DISTINCT mdate, team1, SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) score1,
team2, SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) score2
FROM game
game LEFT JOIN goal ON (id = matchid)
GROUP BY id, mdate, team1, team2
ORDER BY mdate, matchid, team1, team2
```
## More JOIN
### More JOIN [(ENGLISH 1-9)](https://sqlzoo.net/wiki/More_JOIN_operations)
1. List the films where the yr is 1962 Show id, title
```sql
SELECT id, title
FROM movie
WHERE yr=1962
```
2. Give year of 'Citizen Kane'.
```sql
SELECT yr
FROM movie
WHERE title = 'Citizen Kane'
```
3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.
```sql
SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr
```
4. What are the titles of the films with id 11768, 11955, 21191
```sql
SELECT title
FROM movie
WHERE id IN (11768, 11955, 21191)
```
5. What id number does the actor 'Glenn Close' have?
```sql
SELECT id
FROM actor
WHERE name = 'Glenn Close'
```
6. What is the id of the film 'Casablanca'
```sql
SELECT id
FROM movie
WHERE title = 'Casablanca'
```
7. Obtain the cast list for 'Casablanca'.
what is a cast list?
Use movieid=11768 this is the value that you obtained in the previous question.
```sql
SELECT name
FROM casting
JOIN actor ON (id=actorid)
WHERE movieid=11768
```
8. Obtain the cast list for the film 'Alien'
```sql
SELECT name
FROM casting
JOIN actor ON (actor.id=actorid)
JOIN movie ON (movie.id=movieid)
WHERE title = 'Alien'
```
9. List the films in which 'Harrison Ford' has appeared
```sql
SELECT title
FROM casting
JOIN movie ON (movie.id = movieid)
JOIN actor ON (actor.id = actorid)
WHERE name = 'Harrison Ford'
```
10. List the films where 'Harrison Ford' has appeared - but not in the star role.
Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role
```sql
SELECT title
FROM casting
JOIN movie ON (movie.id = movieid)
JOIN actor ON (actor.id = actorid)
WHERE name = 'Harrison Ford'  AND ord > 1
```
11. List the films together with the leading star for all 1962 films.
```sql
SELECT title, name
FROM movie 
JOIN casting ON (id=movieid)
JOIN actor ON (actor.id = actorid)
WHERE ord=1 AND  yr = 1962
```
## Using NULL
### Using NULL [(ENGLISH 1-10)](https://sqlzoo.net/wiki/Using_Null)
1. List the teachers who have NULL for their department.
```sql
SELECT name
FROM teacher
WHERE dept IS NULL
```
2. Note the INNER JOIN misses the teacher with no department and the department with no teacher
```sql
SELECT teacher.name, dept.name
FROM teacher INNER JOIN dept
ON (teacher.dept=dept.id)
```
3. Use a different JOIN so that all teachers are listed.
```sql
SELECT teacher.name, dept.name
FROM teacher LEFT JOIN dept
ON (teacher.dept=dept.id)
```
4. Use a different JOIN so that all departments are listed.
```sql
SELECT teacher.name, dept.name
FROM teacher RIGHT
JOIN dept
ON (teacher.dept=dept.id)
```
5. Use COALESCE to print the mobile number. Use the number '07986 444 2266' there is no number given. Show teacher name and mobile number or '07986 444 2266'
```sql
SELECT name,
COALESCE(mobile, '07986 444 2266')
FROM teacher
```
6. Use the COALESCE function and a LEFT JOIN to print the name and department name. Use the string 'None' where there is no department.
```sql
SELECT teacher.name, COALESCE(dept.name,'None')
FROM teacher
LEFT JOIN dept 
ON teacher.dept = dept.id
```
7. Use COUNT to show the number of teachers and the number of mobile phones.
```sql
SELECT COUNT(name), COUNT(mobile)
FROM teacher
```
8.Use COUNT and GROUP BY dept.name to show each department and the number of staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.
```sql
SELECT dept.name, COUNT(teacher.dept)
FROM teacher
RIGHT JOIN dept ON dept.id = teacher.dept
GROUP BY dept.name
```
9. Use CASE to show the name of each teacher followed by 'Sci' if the the teacher is in dept 1 or 2 and 'Art' otherwise.
```sql
SELECT name, CASE WHEN dept IN (1,2) THEN 'Sci'
ELSE 'Art'
END
FROM teacher
```
10.Use CASE to show the name of each teacher followed by 'Sci' if the the teacher is in dept 1 or 2 show 'Art' if the dept is 3 and 'None' otherwise.
```sql
SELECT teacher.name,
CASE
WHEN dept.id = 1 THEN 'Sci'
WHEN dept.id = 2 THEN 'Sci'
WHEN dept.id = 3 THEN 'Art'
ELSE 'None' END
FROM teacher LEFT JOIN dept ON (dept.id=teacher.dept)
```
## Self JOIN
### Self JOIN [(ENGLISH 1-9)](https://sqlzoo.net/wiki/Self_join)
1. How many stops are in the database.
```sql
SELECT COUNT(*)
FROM stops
```
2. Find the id value for the stop 'Craiglockhart'
```sql
SELECT id
FROM stops
WHERE name = 'Craiglockhart'
```
3. Give the id and the name for the stops on the '4' 'LRT' service.
```sql
SELECT id, name
FROM stops
JOIN route ON id=stop
WHERE company = 'LRT' AND num=4
```
4. The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query and notice the two services that link these stops have a count of 2.
Add a HAVING clause to restrict the output to these two routes
```sql 
SELECT company, num, COUNT(*) AS visits
FROM route WHERE stop=149 OR stop=53
GROUP BY company, num
HAVING visits=2
```
5. Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.
```sql
SELECT a.company, a.num, a.stop, b.stop
FROM route a 
JOIN route b ON (a.company=b.company AND a.num=b.num)
WHERE a.stop=53 AND b.stop = (SELECT id FROM stops WHERE name = 'London Road')
```
6. The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown.
If you are tired of these places try 'Fairmilehead' against 'Tollcross'
```sql
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a
JOIN route b ON(a.company=b.company AND a.num=b.num)
JOIN stops stopa ON (a.stop=stopa.id)
JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' and stopb.name = 'London Road'
```
7. Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith')
```sql
SELECT a.company, a.num  
FROM route a, route b
WHERE a.num = b.num AND (a.stop = 115 AND b.stop = 137)
GROUP BY num;
```
8. Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'
```sql
SELECT a.company, a.num
FROM route a
JOIN route b ON (a.company = b.company AND a.num = b.num)
JOIN stops stopa ON a.stop = stopa.id
JOIN stops stopb ON b.stop = stopb.id
WHERE stopa.name = 'Craiglockhart'
AND stopb.name = 'Tollcross';
```
9. Give a distinct list of the stops which may be reached from 'Craiglockhart' by taking one bus, including 'Craiglockhart' itself. Include the company and bus no. of the relevant services.
```sql
SELECT DISTINCT name, a.company, a.num
FROM route a
JOIN route b ON (a.company = b.company AND a.num = b.num)
JOIN stops ON a.stop = stops.id
WHERE b.stop = 53;
```
