# Working with RDBMS

Interacting with databases - executing select / update / DML statements against a relational database
system - is a special case of performing side effects. Therefore we can give such pieces of code the same
treatment as we gave `Try0<A, E>` in the previous chapter. The trick, again, is that instead of performing
the side effects in our method, we write a function which returns a **description** of the side effect.

These descriptions can be transformed to new descriptions via `map` and `bind`, and we have 
the flexibility to delay their execution until *'the end of the world'*, and execute them in
different manner depending on our use case.

## `fj.control.DB`

There exists a specialized side effect description for writing `jdbc` code. It is called `fj.control.DB`.
We will work with it.

## `sane-dbc`

There exists an [open source library](https://github.com/novarto-oss/sane-dbc)
providing out-of-the-box implementations of `DB` for select, update, batch update,
select unique, transact, etc. We will utilize that library, so that we do not have to write the 
`jdbc` boilerplate ourselves.

Have a look at the tutorial to get started. You are already familiar with the major idea from the
previous chapter, so you should be able to get up to speed easily.

> `sane-dbc` is based on `SQL` and, to a certain extent, `jdbc`; the documentation assumes basic 
familiarity with those. 

## `sane-dbc` links

- [Home Page / Tutorial](https://github.com/novarto-oss/sane-dbc)
- [Gradle project with example code](https://github.com/novarto-oss/sane-dbc/tree/master/sane-dbc-examples)
    - [Example usage of basic operators](https://github.com/novarto-oss/sane-dbc/blob/master/sane-dbc-examples/src/test/java/com/novarto/sanedbc/examples/BasicUsage.java)
- [An introductory presentation to sane-dbc](https://gitpitch.com/dimitarg/talks/master?p=sane-dbc)

## Exercise

A user has an auto-generated ID, a non-null display name, a non-null, unique e-mail and a non-null password.
The password is stored in the database as a one way hash.


Implement a DAO with the following capabilities:

- create the users table
- create a new user
- update a given user's display name
- select user by id
- select user by email
- verify given credentials

We will build on this example later on, to build a realistic, if simple, user management system.


