## SQLZOO_Solutions

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
### SELECT basics [(CHINESE 11-13)](https://sqlzoo.net/wiki/SQLZOO:SELECT_from_WORLD_Tutorial/zh)
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
13.
Put the continents right...
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
