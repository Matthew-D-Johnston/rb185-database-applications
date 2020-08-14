##### RB185 Database Applications

---

## Lesson 2: Database-backed Web Applications

### What to Focus On

* What schema does an application require?
* Project setup is secondary.

---

### Designing a Schema

* Create a `schema.sql` file.

* Write the SQL code and save it to the file.

* Create a new database; then load the file into the database with `psql`

  ```sql
  createdb db_name
  psql -d db_name < schema.sql
  ```

---

