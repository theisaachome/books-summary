## **Introduction**

- [Table of Contents](#Table-of-contents)
- [Relational Databases](#Relational-Databases)
- [Object-Relational Mapping](#Object-Relational-Mapping)

---

### **Introduction**

---

### **Relational Databases**

- Relational stores remain the most popular general purpose databases in existence.

- They are where the vast majority of the world’s corporate data is stored.

- Understanding relational data is key to successful enterprise development.

- Working well with database systems is a commonly acknowledged hurdle of software development.

---

### **Object-Relational Mapping**

- The domain model has a class.
- The database has a table.
- To make the two models talk to each other.

  - writing another Data Access Object (DAO)
  - to convert Java Database Connectivity (JDBC)
  - result sets into something object-oriented.

- Bridge between the gap of the object model and the relational model
  is :

  - **`object-relational mapping `**,
  - referred to as O-R mapping or
  - simply ORM.

### **A brief manifesto of what the ideal solution should be :**

- **Objects, not tables:**

  - Apps are written in the domain model,
  - not bound to the relational model.
  - must operate on and query against the domain model
  - Not in the relational language of tables, columns, and foreign keys.

- **Convenience, not ignorance:**

  - Mapping tools should be familiar with relational technology.
  - O-R mapping is not meant to save developers from understanding mapping problems or to hide them altogether.
  - It is meant for those who have an understanding of the issues and know what they need.
  - No need to write thousands of lines of code to deal with a problem that has already been solved.

- **Unobtrusive, not transparent:**

  - It is unreasonable to expect that persistence be transparent

  - An application needs to have control of the objects

  - that it is persisting and be aware of the entity life cycle.

  - should not intrude on the domain model

  - domain classes must not be required to extend classes

  - or implement interfaces in order to be persistable.

- **Legacy data, new objects:**

  - target an existing relational database schema than create a new one.

  - Support for legacy schemas

  - databases will outlive every one of us.

- **Enough, but not too much:**

  - need features sufficient to solve problems.

  - avoid a heavyweight persistence model that introduces large overhead

  - because it is solving problems that many do not even agree are problems.

- **Local, but mobile:**

  - Data does not need to be modeled as a full-fledged remote object.

  - Distribution is something that exists as part of the application,

  - not part of the persistence layer.

  - The entities must be able to travel to whichever layer needs them

  - so that if an application is distributed,

  - then the entities will support and not inhibit a particular architecture.

- **Standard API, with pluggable implementations:**

  - don’t want to risk being coupled to product-specific libraries and interfaces.

  - By depending only on defined standard interfaces,

  - the application is decoupled from proprietary APIs and

  - can switch implementations if another becomes more suitable.
