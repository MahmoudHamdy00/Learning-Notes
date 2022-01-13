# Advanced Database

## Chapter 14 -> Basics of Functional Dependencies and Normalization for Relational Databases

## 14.1 Informal Design Guidelines

- Guideline 1. Design a relation schema so that it is easy to explain its meaning. Do not combine attributes from multiple entity types and relationship types into a single relation.

  - Making sure that the semantics of the attributes is clear in the schema

- Guideline 2. Design the base relation schemas so that no insertion, deletion, or modification anomalies are present in the relations. If any anomalies are present,note them clearly and make sure that the programs that update the database will operate correctly.

  - Reducing the redundant information in tuples

- Guideline 3. As far as possible, avoid placing attributes in a base relation whose values may frequently be NULL. If NULLs are unavoidable, make sure that they apply in exceptional cases only and do not apply to a majority of tuples in the relation.

  - Reducing the NULL values in tuples

- Guideline 4. Design relation schemas so that they can be joined with equality conditions on attributes that are appropriately related (primary key, foreign key) pairs in a way that guarantees that no spurious tuples are generated.
  - Disallowing the possibility of generating spurious tuples.

## 14.2 Functional Dependencies
