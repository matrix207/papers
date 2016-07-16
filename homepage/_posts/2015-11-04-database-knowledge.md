---
layout: post
title: "Database knowledges"
description: ""
category: database 
tags: [database]
---

### Reference
* `DDL` is Data Definition Language (DDL) statements are used to define the database  
  structure or schema. Some examples:  

    CREATE - to create table in the database
    ALTER - alters the structure of the database
    DROP - delete objects from the database
    TRUNCATE - remove all records from a table, including all spaces allocated for the records are removed
    COMMENT - add comments to the data dictionary
    RENAME - rename an object 

* `DML` is Data Manipulation Language (DML) statements are used to managing data  
  with schema objects. Some examples:  

    SELECT - retrieve data from the a database
    INSERT - insert data into a table
    UPDATE - updates existing data within a table
    DELETE - deletes all records from a table, the space for the records remain
    MERGE - UPSERT operation (insert or update)
    CALL - call a PL/SQL or Java subprogram
    EXPLAIN PLAN - explain access path to data
    LOCK TABLE - control concurrency 

* `DCL` is Data Control Language (DCL) statements.
* `TCL` is Transaction Control Language (DCL) statements.

### 事务
* ACID
  + Atomic 原子性
  + Con 一致性
  + Isolation 隔离性
  + D 持久性
* 并发场景
  + 脏读:
  + 不可重复读
  + 幻读

### Index
* structure
  + B-/B+ tree

### Reference
* [数据库中的DML，DCL，DDL分别是那些操作？](http://liyuan2005.iteye.com/blog/209218)
