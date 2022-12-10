---
description: Some golang concepts from various sources.
---

# Concepts

### Data Semantics

There are 2 types of data semantics in Go. Go is a value semantics oriented language.

#### Value semantics (Value of data is copied & modified in its individual goroutine)

* Every single function call/ goroutine that transitions from 1 program boundry to another is going to operate on its own copy of the data.
* This minimzes side effects & mutation because data is isolated.
* This gives us the highest levels of data integrity

#### Pointer semantics (Value of data is referenced & modified. Data stays in 1 block)

* More efficient than value semantic
* We have to manage only 1 copy of the data
