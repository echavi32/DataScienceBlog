---
title: "Filtering Rows/Columns Using SQL"
linktitle: filtering with SQL
date: '2014-03-10'
prev: /tutorials/mathjax
menu:
  main:
    name: SQL Filtering
weight: 10
---

## Set Up

```{r warning=FALSE, message=FALSE}
library(Lahman)
library(sqldf)
```

##A General Lay Out

Suppose we want to see the homerun totals for the 1927 Yankees. We could write the following:

```{r}
query<-"SELECT playerID,yearID,teamID,HR FROM Batting 
WHERE teamID='NYA'  and yearID=1927"
sqldf(query)
```

##Setting Limits and Data Layout

Suppose you want to find all instances where Yankees have hit 40 homeruns or more.

```{r}
query<-"SELECT playerID,yearID,teamID,HR FROM Batting
WHERE HR>39"
sqldf(query)
```

##Setting Right and Left Limits To Data Set

Suppose we want to now find all examples where a player had more than 40 homeruns but less than 60 strikeouts.

```{r}
query<-"SELECT playerID,yearID,teamID,HR,SO FROM Batting
WHERE HR>39 and SO<60"
sqldf(query)
```

##Setting A Starting Limit, With Ordering By

Again, you want to find all instances of a player striking out less than 10 times. At least 400 at-bats (AB) players with least strikeouts at the top.

```{r}
query<-"SELECT playerID,yearID,teamID,SO,AB FROM Batting
WHERE AB>399 and SO<10"
sqldf(query)
```

##Ordering By, Different Scenario

Finding every instance of a player hitting more than 50 homeruns but lets have the players with the most homeruns at the top.

```{r}
query<-"SELECT playerID,yearID,teamID,HR FROM Batting
WHERE HR>50
ORDER BY HR DESC"
sqldf(query)
```

Finding Babe-Ruth's career homerun totals.

```{r}
query<-"SELECT playerID, sum(HR) FROM Batting
WHERE playerID='ruthba01'
GROUP BY playerID"
sqldf(query)
```

Finding the Career homerun totals of all players, but limit the display to only those that hit more than 600, and having the players with the highest total at the top.

```{r}
query<-"SELECT playerID, sum(HR) FROM Batting
GROUP BY playerID
HAVING sum(HR)>600
ORDER BY sum(HR) DESC"
sqldf(query)
```

Finding the players with the highest homerun average over their career. Limit the display to those who have an average of more than 30. Players withe the highest average at the top.

```{r}
query<-"SELECT playerID, avg(HR) FROM Batting
GROUP BY playerID 
HAVING avg(HR)>30
ORDER BY avg(HR) DESC"
sqldf(query)
```

Finding all instances of players hitting more than 50 homeruns, give first, and last names teamID, yearID, and homeruns


```{r}
query<-"SELECT nameFirst, nameLast, yearID, teamID, HR
FROM Batting INNER JOIN Master
ON Batting.playerID=Master.playerID
WHERE HR>50"
sqldf(query)
```
List firstname, lastname, year, teamID, and HR limit to babe ruth.

```{r}
query<-"SELECT nameFirst, nameLast, yearID, teamID, HR 
FROM Batting INNER JOIN Master
ON Batting.playerID=Master.playerID
WHERE Batting.playerID='ruthba01'"
sqldf(query)
```

baberuth stats playerID, team names, yearID, HR

```{r}
query<-"SELECT nameFirst, nameLast, Batting.yearID, Batting.HR
FROM Batting INNER JOIN Teams INNER JOIN Master
ON Batting.teamID = Teams.teamID AND Batting.yearID=Teams.yearID
WHERE Batting.playerID='ruthba01'
ORDER BY Batting.HR DESC"
sqldf(query)
```

