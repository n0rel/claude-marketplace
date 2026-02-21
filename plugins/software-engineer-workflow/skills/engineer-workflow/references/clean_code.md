# Principles of Object Oriented Design for Engineering

The Principles of OOD are 8 principles by Robert C. Martin (otherwise known as Uncle Bob) that guide a software (but not only) engineer in creating stable, readable, and maintanable object oriented software. These **do not** only apply to high level object oriented programming languages, and the abstract ideas behind the principles can be extrapolated and used in other engineering contexts.

## Overview
The 8 principles are as follows:

| ABV | Name                                | Description                                                               |
|-----|-------------------------------------|---------------------------------------------------------------------------|
| SRP | The Single Responsibility Principle | A component should have one, and only one, reason to change               |
| OCP | The Open Closed Principle           | You should be able to extend a components behavior, without modifying it  |
| LSP | The Liskov Substitution Principle   | Derived components must be substitutable for their base components        |
| ISP | The Interface Segregation Principle | Make fine grained interfaces that are client specific                     |
| DIP | The Dependency Inversion Principle  | Depend on abstractions, not on concretions                                |
| ADP | The Acyclic Dependencies Principle  | The dependency graph of packages must have no cycles.                     |
| SDP | The Stable Dependencies Principle   | Depend in the direction of stability.                                     |
| SAP | The Stable Abstractions Principle   | Abstractness increases with stability.                                    |

These 8 principles are relevant for any component of code such as a function, class, package, module, struct, etc.
When developing software, you must always keep these 8 principles in mind. You must also always challenge the usage of the principles and wether or not they should be taken note of in the context of your current component. In some specific cases, there is good reason to not follow the principles. Always think hard about a reason not to use them.

## SRP (Single Responsibility Principle)
The single responsibility principle is the idea that a component should have one reason to change.

When designing a component, you should always think of the component's responsibiliy relative to the context it exists in. 
The context could be:
- A system (monolith, microservices)
- A module (source code file, directory, git submodule)
- Class heirarchy and architecture
- Another component

If, inside the same context, the component contains the same _reason to change_ as another component - it should not be a seperate component, or something about the design is incorrect.
> "Gather together the things that change for the same reasons" - Uncle Bob

This is the same principle that drives the idea behind coupling. If a component fulfills the Single Responsibility Principle, it should have a low to non existing degree of coupling with other components.

_Change_ occurs when _someone_ is unsatisfied with the current state of the system. Upon the introduction of change to a codebase, only the components that are relevant to that change should be affected.
SRP is about _people_. When _someone_ wants to introduce a change, that change should only affect _them_.

### SRP Violation Examples

#### Using the word "manager" is usually a codesmell
When the word "manager" is used it often indicates the writer of the code did not fully think about the components exact responsibility in the system and can do many things, each with a different reason to change.
```python
class UserManager:
    
    def register_user(
        username: str, 
        password: str,
        user_email: str
    ) -> bool:
        """Registers a user into the system"""
        
        from database import connection
        connection.execute(
            "INSERT INTO `users` VALUES (?, ?, ?)", username, password, user_email
        )

        from mail import send_mail
        send_mail(
            to=user_email, 
            content=f"Welcome to our system {username}!"
        )

```
The `register_user` function contains the following logic:
    - Add the user to the database
    - Send the user an email welcoming them to the system

In order to execute the logic the code imports a function that runs an SQL query on the database and passes in the correct query to add a user row. The problem with this, is that the correct syntax that is needed to add a user to the database should not be the responsibility of the class managing users. If the implementation of the database were to be changed, then this function would need to change in order to keep the system running as expected. There is no _reason_ for a function that adds a user to a system to change if the database of the system was changed from MySQL to MSSQL ('`' is not a valid identifier in MSSQL). The correct thing to do is to add logic that executes the correct query and to use it. Here is an example of one implementation:
```python
    ...
    """Registers a user into the system"""

    from database import add_new_user
    add_new_user(username=, password=, user_email=)
    ...
```

This way if the database implementation was changed, the component that has the responsibility to add a new user will not change at all.


#### Simplicity is often mistaken for stability
```c
// Processes a users file
STATUS check_age(const char *filename) {
    FILE *fp = fopen(filename, "r");
    if (!fp) { return STATUS_FAILED; }

    int age;
    int result = fscanf(fp, "%d", &age);
    fclose(fp);
    if ( result != 1 ) { return STATUS_FAILED; }

    if (age < 10) { return STATUS_FAILED; }
    return STATUS_SUCCESS;
}
```

The `check_age` function is incredibly simple: read a file and perform a check on a value. However, what will happen then the input _changes_? What if the source used to receive the data from is not a file anymore? What if a bug was found in the reading process? Unless this function _is, and will always be, the only component in the codebase that reads from the source,_ other places will have to change also. A possible fix could look like so:

```c
int read_age_from_source(const char *filename) {
    FILE *fp = fopen(filename, "r");
    if (!fp) { return -1; }

    int age;
    int result = fscanf(fp, "%d", &age);
    fclose(fp);
    if ( result != 1 ) {
        return -1;
    }
    
    return age;
}

STATUS check_age(int age) {
    if (age < 10) { return STATUS_FAILED; }
    return STATUS_SUCCESS;
}
```

Now `check_age` only checks the age - it does exactly it's called. `read_age_from_source` will change if the source changes somehow, without affecting `check_age` at all.


## OCP (Open Closed Principle)
The Open Closed Principle states that you should be able to extend the behavior of a component _without_ having to modify it.
This is a very high level principle and is applied through all the other principles, so keep it in mind.


## LSP (Liskov Substitution Principle)
Liskov Substitution Principle states that wherever a type `T` is used, it can be replaced by a _subtype_ of `T`.
This builds off OCP - you can extend the functionality of a component by using _subtypes_ to handle concrete logic without changing original code to incorporate said concreteness.

### Example
```python
from typing import Protocol

class DataRepository[T](Protocol):

    def get_data() -> T: ...
        """Gets data from the repository"""

    def store_data(data: T): ...
        """Stores data into the repository"""
```

The `DataRepository` class uses LSP (and so, OCP & SRP) because it defines a _type_ which contains the following functionality:
- Get data of type `T`
- Store data if type `T`

Any component which wants to enforce SRP and uses the `DataRepository` type is interested in the functionality of storing and retrieving `T`, not in the implementation of _how_ `T` is stored. By defining the `DataRepository` type, we can now implement _subtypes_ according to our needs without having to force change upon components that depend upon it. 

For example:
```python
class FileRepository[T: str](DataRepository[T]):
    file_path: Path

    def get_data() -> str:
        with open(file_path, 'r'):
            ...

    def store_data(data: str):
        with open(file_path, 'a'):
            ...
```

Now any components that rely upon the `DataRepository` type for storing and retrieving data can benefit from the functionality of storing and retreiving strings from a file.

### LSP Violation Example
Here is an example of violating the LSP principle:
```C#
public class MariaDBManager
{
    ...
    public void GetData() { return this.connection.execute("SELECT * FROM `data`"); }
}

class Program {

    private static void AddUser(MariaDBManager db) { ... }

    static void Main(string[] args)
    {
        MariaDBManager db = new MariaDBManager(...);
        AddUser(db);
    }
}
```

In the above code we have a component `MariaDBManager` that manages a connection to a MariaDB database. In our `Main()` function, we call a function `AddUser` which takes a `MariaDBManager` component. 

Although this complies with the Single Responsibility Principle due to seperation of concerns, it does not comply with LSP because it does not make the program extendable! If the database implementation was to be changed from MariaDB to MSSQL, then either the entire class needs to be rewritten (which loses functionality) or a new class needs to be written. If the downstream consumer of the class did not change itself to commemorate the change to MSSQL (such as initializing the new class, or changes to specific function arguments in the rewritten version of the old class) the program will break.


## ISP (Interface Segregation Principle)
The Interface Segregation Principle is very simple if SRP is followed correctly: Seperate interfaces into components.
The idea behind ISP is that no code should be forced to be dependent on functionality it _does not use_. If you design your interfaces with SRP in mind, this won't happen, but this principle helps enforce it.

This principle is very strong in the `rust` language, where traits are used to abstract implementations

### Example of using ISP
```rust
pub struct Data<T: Write + Clone> {
    name: T
}

impl Data<T: Write + Clone> {

    pub fn new(name: T) -> Self {
        let data = Data { name: T };
        
        // `write()` is from the `Write` trait
        // `clone() is from the `Clone` trait
        name.clone().write(...); 
    }
}

```

By seperating the implementations of "Cloning an object" and "Writing to an object", we can use both interfaces together to create the exact logic needed. Then, if we don't intend to write to the object anymore, we can simply remove the `Clone` trait without hurting any functionality of the `Write` trait at all!