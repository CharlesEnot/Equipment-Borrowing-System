# Equipment Borrowing System

## About the Project

This is our laboratory activity for **ITSD 81 – Desktop Application Development**.

The project is a simple **Campus Equipment Borrowing System**. The purpose of the system is to allow students to borrow equipment if the equipment is available and the student is allowed to borrow.

We made this project using **C# and .NET**.

There is no database or graphical user interface yet because it is not required for this activity.

---

## 1. Solution Structure

Our project is separated into different parts so that the code is not all mixed together.

```text
EquipmentBorrowing/
│
├── README.md
├── EquipmentBorrowing.sln
│
├── src/
│   ├── EquipmentBorrowing.Domain/
│   ├── EquipmentBorrowing.Application/
│   ├── EquipmentBorrowing.Infrastructure/
│   └── EquipmentBorrowing.ConsoleDemo/
│
└── tests/
    └── EquipmentBorrowing.Tests/
```

### Domain

The Domain project contains the main things in the system.

Some of the classes are:

* Student
* Equipment
* Borrowing
* BorrowingStatus

This is where we put information and rules that are related to the actual borrowing system.

### Application

The Application project contains the services that do the actual operations.

For example:

* BorrowEquipmentService

The service checks the student, equipment, and borrowing information before creating a borrowing.

### Infrastructure

The Infrastructure project contains the repository implementations.

For this activity, we used **in-memory repositories** instead of a real database.

This means the data is just stored while the program is running.

### Tests

The Tests project contains some automated tests to check if the system is working correctly.

---

## 2. Dependency Direction

The projects are separated so they don't all depend on each other randomly.

The basic idea is:

```text
Console Demo
     |
     v
Application
     |
     v
Domain

Infrastructure
     |
     v
Domain
```

The Application uses repository interfaces instead of directly using the actual repository implementation.

This makes it easier to change the way data is stored later.

---

## 3. Use Case Mapping

### Actor

Student

### Use Case

Borrow Equipment

### Application Service

`BorrowEquipmentService`

### Domain Objects Used

* Student
* Equipment
* Borrowing
* BorrowingStatus

### Repository Interfaces Used

* `IStudentRepository`
* `IEquipmentRepository`
* `IBorrowingRepository`

### Infrastructure Implementations Used

* `InMemoryStudentRepository`
* `InMemoryEquipmentRepository`
* `InMemoryBorrowingRepository`

---

## 4. How Borrowing Works

When a student tries to borrow equipment, the application checks a few things first.

```text
Student requests equipment
          |
          v
Check if student exists
          |
          v
Check if student is allowed
          |
          v
Check if equipment exists
          |
          v
Check if equipment is available
          |
          v
Check active borrowing limit
          |
          v
Create borrowing
```

If everything is okay, the borrowing is created.

If something is wrong, the system returns a failure message instead.

---

## 5. Successful Case

The program demonstrates a successful borrowing.

For example, a student requests equipment that is available and the student is allowed to borrow.

The application then creates the borrowing record.

The console displays that the borrowing was successful.

---

## 6. Failure Case

The program also demonstrates situations where borrowing is not allowed.

Some examples are:

* Equipment does not exist.
* Equipment is already unavailable.
* Student is not allowed to borrow.
* Student already reached the maximum number of active borrowings.

This is useful because the system should not just create a borrowing without checking anything.

---

## 7. Why Use Repository Interfaces?

We used repository interfaces so the application does not need to know exactly where the data is coming from.

Right now we are using in-memory storage.

Later, it could be changed to SQLite or another database without changing the main borrowing service too much.

For example:

```text
Application
     |
     v
IEquipmentRepository
     |
     v
InMemoryEquipmentRepository
```

Later the implementation could be something like:

```text
Application
     |
     v
IEquipmentRepository
     |
     v
SQLiteEquipmentRepository
```

So the application can still use the same interface.

---

## 8. Why We Did Not Use a Database

The instructions said that a database is not required for this activity.

Because of that, we used simple in-memory repositories.

We also did not use Entity Framework Core.

---

## 9. Why We Did Not Use Avalonia

Avalonia UI is also not required yet.

This activity is mainly about creating the structure of the application and separating the responsibilities.

For now, we use a console program to demonstrate that the application works.

If a UI is added later, the Avalonia views would be part of the UI/executable layer instead of putting UI code inside the Domain or Application projects.

---

## 10. Reflection

### 1. Why should the application service depend on a repository interface instead of directly depending on a database implementation?

Because the application service should not care about how the data is stored.

Using an interface makes it possible to change the repository later without changing the service.

### 2. Which parts of the current solution could remain unchanged if SQLite were added later?

The Domain classes and most of the Application code could stay the same.

The repository implementations in Infrastructure would be changed or replaced to use SQLite.

### 3. Which project would eventually contain Avalonia Views?

The UI project would contain the Avalonia Views.

It should not be placed inside the Domain project.

### 4. Should an Avalonia button directly execute database queries? Why or why not?

No.

The button should call the application logic instead.

The application service can then use the repository to get or save data.

This keeps the UI and database code separate.

### 5. What part of the implementation represents the actual business operation requested by the actor?

The `BorrowEquipmentService` represents the main borrowing operation.

It checks the required conditions and creates the borrowing when everything is valid.

---

## 11. Running the Project

To build the solution:

```bash
dotnet build
```

To run the tests:

```bash
dotnet test
```

To run the console demonstration:

```bash
dotnet run --project src/EquipmentBorrowing.ConsoleDemo
```

The console demo shows both successful and unsuccessful borrowing attempts.

---

## 12. Conclusion

This project is only the basic structure of the equipment borrowing system.

It demonstrates how we separated the Domain, Application, Infrastructure, and Tests instead of putting everything in one file.

A database and graphical interface can be added later.

For this laboratory activity, the important part is that the solution is organized, the borrowing operation works, and the application can be changed later without rewriting everything.
