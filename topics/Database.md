# What is ACID compliance?

The presence of four components — atomicity, consistency, isolation and  durability — can ensure that a database transaction is completed in a timely manner. When databases possess these components, they are said to be ACID-compliant.

## Atomicity

Database transactions, like atoms, can be broken  down into smaller parts. When it comes to your database, atomicity  refers to the integrity of the entire database transaction, not just a component of it. In other words, if one part of a transaction doesn’t  work like it’s supposed to, the other will fail as a result—and vice versa

## Consistency

For any database to operate as it’s intended to  operate, it must follow the appropriate data validation rules. Thus,  consistency means that only data which follows those rules is permitted  to be written to the database. If a transaction occurs and results in  data that does not follow the rules of the database, it will be ‘rolled  back’ to a previous iteration of itself (or ‘state’) which complies with the rules. On the other hand, following a successful transaction, new  data will be added to the database and the resulting state will be  consistent with existing rules.

## Isolation

For a database, isolation refers to the ability to  concurrently process multiple transactions in a way that one does not  affect another.

## Durability

In databases that  possess durability, data is saved once a transaction is completed, even  if a power outage or system failure occurs.

# Resources

-   [What is ACID Compliance](../sources/What is ACID Compliance/What is ACID Compliance.md)

