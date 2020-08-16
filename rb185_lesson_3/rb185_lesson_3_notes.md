##### RB185 Database Applications

---

## Lesson 3: Optimization

### What to Focus On

* Optimization is something to be aware of.

---

### Summary

In this lesson, we learned about:

* How N+1 queries are the result of performing an additional query for each element in a collection
* How to move business logic from Ruby into the database by adding to a query's select list
* How making database interactions more efficient often involves making SQL queries more specialized

---

### Quiz Lesson 3

1. The following are four ways that we can optimize the way an application interacts with a database:

   i) Making the SQL queries that the application uses less general and more specialized.

   ii) Pushing some aspects of the application logic (e.g. counting records) down to the database.

   iii) Reducing the number of queries that an application needs to make.

   iv) Analyzing and comparing the costs of running different SQL statements to identify which are the most efficient for the application to use.

2. What do we mean by an n + 1 query?

   * It's where an application issues one query to retrieve data for a parent record, and then an additional query for each child record assocaited with that parent.



