# Chapter 2: Theory: Yes, We Need It!
- necessary to understand the operations used by the db engine as it interprets a query and the underlying theory

## Query Processing Overview
postgres does the following: 
- compile and transform sql statement into expression consisting of high-level logical operations (aka logical plan)
- optimize the logical plan and convert it into an execution plan
- execute (interpret) the plan and return results 

## Compilation 
- same as imperative language: parse source code, generate internal representation
- a couple differences
     - in imperative languagesdefinitions of identifiers are included in source code, versus in sql where definitions of objects are mostly stored in the db which means that different servers can interpret the same query differently because the meaning of a query depends on the db structure
     - also the output of imperative language compiler is usually (almost) executable, versus the output of a query compiler is an expression consisting of high-level operations that remain declarative (they do specify an order of operations but not the manner of execution of those operations)

## Optimization and Execution
- instructions about how to execute the query appear at the next phase of query processing, optimization
- two kinds of transformations: replacing logical expressions with their execution algorithm and possibly changing the logical expression structure by changing their order
- neither is straightforward: the optimizer tries to, well, optimize… and how this happens is out of scope of this particular book
- output of optimizer is expression containing physical operations, called a physical execution plan (the postgres optimizer is called the query planner)
- finally, the query execution plan is interpreted by the query execution engine and output is returned to the client application	

## Relational, Logical, and Physical Operations
-	we must understand how relational operations correspond to logical operations and the query language used in queries

## Relational Operations
-	central concept of relational theory is, you guessed it, a relation. for the purposes of this study we’re going to view a relation as a table and not quibble academically about what a relation actually is
-	any relational operation takes one or more relations as its arguments and produces another relation as its output, which can again be used as an argument for another relational operation
-	the ability to construct complex expressions makes the set of relational operations (aka relational algebra) a powerful query language
-	expressions in relational algebra can be used to define additional operations 

the first three operations are filter, project, and product 

-	filter: selection (called restriction in relational theory)
	accepts a single relation as an arg and includes in output all tuples that satisfy condition specified as a filtering condition
	
-	project: takes single relation and removes some attributes AND ALSO REMOVES DUPS unlike the sql project operation (to make it distinct you must add distinct)

-	product: produces the set of all pairs of rows from first and second args
	very difficult to find a useful real-life example of a product 
	but we can imagine that we want to find all flights from all airports in the entire world to any other airport

	after having covered these you may feel cheated: where was join, in that list? 
	join is a product followed by a filter
	
-	other relational operations include grouping, union, intersection, and set differences 
		
-	equivalence rules are the last piece of relational theory we need to know for optimization purposes
-	commutativity means that the order of two relations is not necessarily important
-	associativity means that if we have three relations, S, R, and T, we can choose to first perform R JOIN S and then JOIN T to the result or S JOIN T and then JOIN R to the result of that
-	distributivity means that if we are joining a relation with a UNION of two other relations, the result is the same as two original joins followed by a UNION of the resulting outputs
	many more equivalence rules

## Logical Operations
-	set of logical operations needed for representation of SQL queries includes all relational operations but the semantics is different: e.g. SQL project operation doesn’t remove dups 
-	left, right, and full outer joins all produce results that are not technically relations but are tables 

## Queries as Expressions: Thinking in Sets
-	the ability of optimizer to produce efficient plan relies on two things: rich set of equivalences provides for a large space of equivalent expressions
-	relational operations produce no side effects, the only thing produced is the result of the operation (this is mentioned because sometimes developers think they need intermediate storage for results of intermediate operations but this produces unneeded overhead and can block the efficiency of the optimizer)	

## Operations and Algorithms
-	the query planner in postgres is what replaces logical operations with physical operations
-	when we move from logical to physical level, mathematical relations are stored in tables which are stored in the db, and we must identify ways to retrieve data from those tables
-	stored data must be extracted with one of the few data access algorithms in next chapter (usually data access algorithms are combined with operations that consume their results)
-	more complex logical operations (join, union, grouping) can be done with alternative algorithms (complex logical operations can sometimes be replaced by several physical operations)
