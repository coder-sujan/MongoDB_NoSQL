# MongoDB_NoSQL
MongoDB_NoSQL Some ideas to start

## Core Concepts

Database → contains collections

Collection → contains documents

Document → JSON-like object (actually BSON on disk)

Flexible schema; documents in the same collection can have different fields.


| SQL      | MongoDB    |
| -------- | ---------- |
| Database | Database   |
| Table    | Collection |
| Row      | Document   |
| Column   | Field      |



## Create / pick DB & collection

- use school                           // switch to (or create) 'school'
- db.createCollection("students")      // optional; insert will also create it
- db.getName()                         // confirm active DB
- db.getCollectionNames()              // list collections


## Seed data (with varied types)

db.students.deleteMany({}) // reset for demo

- db.students.insertMany([
  { name: "Alice",   age: 20, course: "Math",      skills: ["Python","Stats"],   gpa: 3.4, joined: ISODate("2024-03-01") },
  { name: "Bob",     age: 21, course: "Physics",   skills: ["Lab","Calculus"],   gpa: 3.7, joined: ISODate("2023-07-15") },
  { name: "Charlie", age: 23, course: "Chemistry", skills: ["Organic","Safety"], gpa: 3.1, joined: ISODate("2022-11-20") },
  { name: "Dina",    age: 22, course: "CS",        skills: ["Java","MongoDB"],   gpa: 3.9, joined: ISODate("2025-02-10") },
  { name: "Evan",    age: 24, course: "CS",        skills: ["MongoDB","Python"], gpa: 2.9, joined: ISODate("2021-09-05") },
  { name: "Farah",   age: 22, course: "Math",      skills: ["Algebra","Python"], gpa: 3.5 }
])

## READ (filters, projection, sort)

> db.students.find()                                                 // all docs

> db.students.find({}, { name:1, course:1, _id:0 }).limit(5)         // projection + limit

> db.students.find({ course: "CS" })                                 // equality

> db.students.find({ age: { $gt: 21 } })                             // comparison

> db.students.find({ skills: "MongoDB" })                            // array contains value

> db.students.find({ $or: [ { course: "Physics" }, { age: { $lt: 21 } } ] }) // OR

> db.students.find().sort({ age: -1 }).limit(3)                      // sort desc


--- 

# CRUD Operation

### CREATE (insert)

> db.students.insertOne({
  name: "Grace",               // string
  age: 20,                     // number
  course: "Math",              // string
  skills: ["Python","Algebra"],// array
  address: { city: "Sydney" }  // nested document
})


### UPDATE (set, inc, addToSet)

> db.students.updateOne(
  { name: "Alice" },                       // filter
  { $set: { age: 21, email: "alice@ex.com" } } // change fields (create if absent)
)


> db.students.updateMany(
  { course: "Physics" },
  { $set: { status: "lab-required" } }
)

> db.students.updateMany(
  { course: "Chemistry" },
  { $inc: { age: 1 } }                     // numeric increment
)


> db.students.updateOne(
  { name: "Dina" },
  { $addToSet: { skills: "Data Structures" } } // add to array if not present
)

### DELETE

> db.students.deleteOne({ name: "Bob" })              // remove one
>> db.students.deleteMany({ age: { $lt: 21 } })        // remove many (age < 21)
>>> db.students.countDocuments()                        // check remaining
