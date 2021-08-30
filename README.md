## SQLZOO_Solutions

## Sections:
1. [SELECT basics](#select-basics)
2. [SELECT from WORLD](#select-from-world)
3. [SELECT from NOBEL](#select-from-nobel)
4. [SELECT within SELECT Tutorial](#select-within-select-tutorial)

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

