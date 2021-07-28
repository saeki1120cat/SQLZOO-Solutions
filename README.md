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

