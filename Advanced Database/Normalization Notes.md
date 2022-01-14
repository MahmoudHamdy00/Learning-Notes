# Advanced Database

## Chapter 14 - Basics of Functional Dependencies and Normalization for Relational Databases

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

A formal tool for analysis of relational schemas that enables us to detect and describe some of the above-mentioned problems in precise terms.

A functional dependency is a constraint between two sets of attributes from the database.

**Definition.** A functional dependency, denoted by X → Y, between two sets of attributes X and Y that are subsets of R(R = {A1, A2, … , An}) specifies a constraint on the possible tuples that can form a relation state r of R. The constraint is that, for any two tuples t1 and t2 in r that have t1[X] = t2[X], they must also have t1[Y] = t2[Y].

This means that the values of the Y component of a tuple in r depend on, or are determined by, the values of the X component; alternatively, the values of the X component of a tuple uniquely (or functionally) determine the values of the Y component.

- If a constraint on R states that there cannot be more than one tuple with a given X-value in any relation instance r(R)—that is, X is a candidate key of R—this implies that X → Y for any subset of attributes Y of R (because the key constraint implies that no two tuples in any legal state r(R) will have the same value of X). If X is a candidate key of R, then X → R.

- If X → Y in R, this does not say whether or not Y → X in R.

**An FD cannot be inferred automatically from a given relation extension r but must be defined explicitly by someone who knows the semantics of the attributes of R.**

we cannot determine which FDs hold and which do not unless we know the meaning of and the relationships among the attributes. All we can say is that a certain FD may exist if it holds in that particular extension. We cannot guarantee its existence until we understand the meaning of the corresponding attributes. We can, however, emphatically state that a certain FD does not hold if there are tuples that show the violation of such an FD.

## 14.3 Normal Forms Based on Primary Keys

Most practical relational design projects take one of the following two approaches:

- Perform a conceptual schema design using a conceptual model such as ER or EER and map the conceptual design into a set of relations.
- Design the relations based on external knowledge derived from an existing implementation of files or forms or reports.

### 14.3.1 Normalization of Relations

The first, second, and third normal form. A stronger definition of 3NF—called Boyce-Codd normal form (BCNF)— All these normal forms are based on a single analytical tool: the functional dependencies among the attributes of a relation.

A fourth normal form (4NF) and a fifth normal form (5NF), based on the concepts of multivalued dependencies and join dependencies, respectively.

Normalization of data can be considered a process of analyzing the given relation schemas based on their FDs and primary keys to achieve the desirable properties of

1. minimizing redundancy
1. minimizing the insertion, deletion, and update anomalies

**Definition.** The normal form of a relation refers to the highest normal form condition that it meets, and hence indicates the degree to which it has been normalized.

### 14.3.2 Practical Use of Normal Forms

**Denormalization** is the process of storing the join of higher normal form relations as a base relation, which is in a lower normal form.

### 14.3.3 Definitions of Keys and Attributes Participating in Keys

Definition. A superkey of a relation schema R = {A1, A2, … , An} is a set of attributes S ⊆ R with the property that no two tuples t1 and t2 in any legal relation state r of R will have t1[S] = t2[S]. A key K is a superkey with the additional property that removal of any attribute from K will cause K not to be a superkey anymore.

Definition. An attribute of relation schema R is called a prime attribute of R if it is a member of some candidate key of R. An attribute is called nonprime if it is not a prime attribute—that is, if it is not a member of any candidate key.

1. Primary key
   A primary key is a column of a table or a set of columns that helps to identify every record present in that table uniquely. There can be only one primary Key in a table.

1. Super Key
   Super Key is the set of all the keys which help to identify rows in a table uniquely. This means that all those columns of a table than capable of identifying the other columns of that table uniquely will all be considered super keys.
   Super Key is the superset of a candidate key (explained below). The Primary Key of a table is picked from the super key set to be made the table’s identity attribute.

1. Candidate Key
   Candidate keys are those attributes that uniquely identify rows of a table. The Primary Key of a table is selected from one of the candidate keys. So, candidate keys have the same properties as the primary keys explained above. There can be more than one candidate keys in a table.

1. Alternate Key
   As stated above, a table can have multiple choices for a primary key; however, it can choose only one. So, all the keys which did not become the primary Key are called alternate keys.

1. Foreign Key
   Foreign Key is used to establish relationships between two tables. A foreign key will require each value in a column or set of columns to match the Primary Key of the referential table. Foreign keys help to maintain data and referential integrity.

1. Composite Key
   Composite Key is a set of two or more attributes that help identify each tuple in a table uniquely. The attributes in the set may not be unique when considered separately. However, when taken all together, they will ensure uniqueness.

1. Unique Key
   Unique Key is a column or set of columns that uniquely identify each record in a table. All values will have to be unique in this Key. A unique Key differs from a primary key because it can have only one null value, whereas a primary Key cannot have any null values.

For more visit [types-of-keys-in-dbms-upgrad](https://www.upgrad.com/blog/types-of-keys-in-dbms/) or [dbms-keys-javatpoint](https://www.javatpoint.com/dbms-keys).

### 14.3.4 First Normal Form

it was defined to disallow multivalued attributes, composite attributes, and their combinations.
It states that the domain of an attribute must include only atomic (simple, indivisible) values and that the value of any attribute in a tuple must be a single value from the domain of that attribute.

_Relation should have no multivalued attributes or nested relations._

### 14.3.5 Second Normal Form

Second normal form (2NF) is based on the concept of full functional dependency.
A functional dependency $X → Y$ is a full functional dependency if removal of any attribute A from X means that the dependency does not hold anymore;

A functional dependency $X → Y$ is a partial dependency if some attribute A ε X can be removed from X and the dependency still holds

**Definition. A relation schema R is in 2NF if every nonprime attribute A in R is fully functionally dependent on the primary key of R.**
_For relations where primary key contains multiple attributes, no nonkey attribute should be functionally dependent on a part of the primary key_

### 14.3.6 Third Normal Form

Third normal form (3NF) is based on the concept of transitive dependency.

A functional dependency $X → Y$ in a relation schema R is a transitive dependency if there exists a set of attributes Z in R that is neither a candidate key nor a subset of any key of R, and both X → Z and Z → Y hold.
_Relation should not have a nonkey attribute functionally determined by another nonkey attribute (or by a set of nonkey attributes). That is, there should be no transitive dependency of a nonkey attribute on the primary key_

**Definition. According to Codd’s original definition, a relation schema R is in 3NF if it satisfies 2NF and no nonprime attribute of R is transitively dependent on the primary key.**

Definition. A relation schema R is in third normal form (3NF) if, whenever a nontrivial functional dependency X → A holds in R, either (a) X is a superkey of R, or **(b) A is a prime attribute of R.**

## 14.5 Boyce-Codd Normal Form

Boyce-Codd normal form (BCNF) was proposed as a simpler form of 3NF, but it was found to be stricter than 3NF. That is, every relation in BCNF is also in 3NF; however, a relation in 3NF is not necessarily in BCNF. We pointed out in the last subsection that although 3NF allows functional dependencies that conform to the clause (b) in the 3NF definition, BCNF disallows them and hence is a stricter definition of a normal form.

Definition. A relation schema R is in BCNF if whenever a nontrivial functional dependency $X → A$ holds in R, then **$X$ is a superkey of R.**

## 14.6 Multivalued Dependency and Fourth Normal Form

A relation schema R is in fourth normal form (4NF) if there isn't a multivalued dependency, $i.e.$ $x → y$ and $x→z$ , y and z are independent.

## 14.7 Join Dependencies and Fifth Normal Form

A relation schema R is in fifth normal form (5NF) it should be in the 4NF and it shouldn't have join dependency.

join dependency is when decompose a table into some tables you should be able to join these tables and construct the original one table, if you couldn't (lost some rows or extra rows will be created ) then don't decompose the table and hence it will be in the 5th normal form (5NF).
