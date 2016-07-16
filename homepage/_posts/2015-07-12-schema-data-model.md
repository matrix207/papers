---
layout: post
title: "schema data model"
description: ""
category: database
tags: [database]
---

### Data model

### Why need it

### Schema Node
* Define
  - R : root node (record node)
  - S : single node (child node, subschema)
  - A : array node (child node, subschema)
* Example
  - R, only root node
  - R -> S1, only a singal node under root
  - R -> A1, only an array node under root
  - R -> A1 -> A2
  - R -> A1 -> S1
  - R -> S1 -> A1
  - R -> S1 -> S2
  - R -> S1 -> A1 -> S2
  - R -> A1 -> S1 -> A2
* How to find the last node info by index of node?
  - step 0, set root node as parent node
  - step 1, use parent node to find array index
  - step 2, found array node, set this node to new parent node, go to step 1
  - step 3, use the parent node to find the last node
  - step 4, do some operations for the node
    + set value to fields and insert. (Using for add a new object)
  - Notices
    + only root node
* How to create a key
* How to write object
* How to read object
* How to update object
* What is difference between full and delta object
* Since there are read and write interfaces, why not insert and delete

### Reference
