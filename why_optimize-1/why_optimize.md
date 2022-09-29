# Chapter 1: Why Optimize 

## What Do We Mean by Optimization?
- any transformation that improves system performance
- not a separate development phase
- how query processing happens is the most important part to understand optimization
- if you practice thinking like the database engine, writing queries correctly the first time and not needing to go back to optimize them becomes second nature

## Why Is It Difficult: Imperative and Declarative
- sql is declarative, we describe the result, not how to get the result 

## Optimization Goals
- “performant query is one that is executed fast” is neither complete nor precise
- what execution time is good enough? 
- user satisfaction depends on response time but is subjective 
- we define the best optimization goals thusly: Specific, Measurable, Achievable, Result-based, Time-bound

## Optimizing Processes 
- the instinct is to over-optimize any bad query that we run across 
- but the most critical optimization step is the first one:determining business intent 

## Optimizing OLTP and OLAP
- Online Transaction Processing vs Online Analytical Processing
- OLTP dbs support applications (short queries)
- OLAP dbs support BY and reporting (short and long queries)

## Database Design and Performance
- optimization starts with requirements gathering 

## Application Development and Performance 
- queries are parts of applications but we can optimize application page response time which is technically not a “database optimization” but will be covered nevertheless in subsequent chapters

## Other Stages of the Lifecycle
- monitoring is necessary throughout application lifecycle 
- important to keep watching your execution times and trends, especially given new releases of postgres, that may prompt query rewriting 

## PostgreSQL Specifics
- unlike other databases, postgres doesn’t have optimizer hints
- it also has a feature called dynamic sql which is often overlooked and will be covered in later chapters…
