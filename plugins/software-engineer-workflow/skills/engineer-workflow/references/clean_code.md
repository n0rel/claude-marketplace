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
| ADP | The Acyclic Dependencies Principle  | The dependency graph of components must have no cycles                    |
| SDP | The Stable Dependencies Principle   | Depend in the direction of stability.                                     |
| SAP | The Stable Abstractions Principle   | Abstractness increases with stability.                                    |

These 8 principles are relevant for any component of code such as a function, class, package, module, struct, etc.
When developing software, you must always keep these 8 principles in mind. You must also always challenge the usage of the principles and wether or not they should be taken note of in the context of your current component. In some specific cases, there is good reason to not follow the principles. Always think hard about a reason not to use them.

## Choosing the Right Level of Rigor
Not every principle **must** be applied to all components to the maximum. Deciding when the principles should be used and how far to take them is part of the process of engineering.
A good guiding question of when to use any principle is as follows: **how many people will be affected if this component changes?** If the answer is _"just me, right now"_ then you are probably in a prototyping phase and change will come fast, and your code needs to be able to change fast. If the answer is _"multiple people over multiple years"_ then these principles can prevent messy codebases and unstable features.

If splitting a component creates 5 new files and 8 new imports for what was 30 lines of cohesive logic, the code has become **less** readable. 
The goal is meaningful separation, not maximum separation.

When in doubt, favor the simpler design and refactor when a real second use case appears.


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

### When NOT to Apply SRP
Do not split a component if the two behaviors are so tightly coupled that separating them creates two components that always change together anyway. For example, serializing an object and validating its fields before serialization are often the same concern. Splitting them into two classes that must always be updated in lockstep is worse than one cohesive component. Also, in a small script or prototype, SRP at the function level is sufficient; you do not need to create class hierarchies.

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
            "INSERT INTO `users` VALUES (?, ?, ?)", 
            username, password, user_email
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
In practice, this means designing components so that new functionality is added by writing new code (new classes, new implementations, new modules) rather than by editing existing, working code. Every time you modify an existing component to handle a new case, you risk breaking the existing cases it already handles.

### Example of Violating OCP
The most common OCP violation is a growing chain of conditionals that must be edited every time a new variant is introduced.

```python
class NotificationService:
    _smtp_client: Email
    _sms_gateway: SMS
    _slack_api: Slack

    def send(self, channel: str, message: str):
        if channel == "email":
            self._smtp_client.send(message)
        elif channel == "sms":
            self._sms_gateway.send(message)
        elif channel == "slack":
            self._slack_api.post(message)
        ...
```
Every time the system needs to support a new notification channel, `send()` must be modified. 
This violates OCP because the existing, working code is opened for editing, which introduces risk to all existing channels.

To fix this, we can create an interface:
```python
from typing import Protocol

class NotificationChannel(Protocol):
    def send(self, message: str) -> None: ...

class EmailChannel:
    def send(self, message: str) -> None:
        self._smtp_client.send(message)
    ...
```
Now we can add more channels without accidentally hurting functionality of another.

In general, the Open Closed Principle shows itself throughout the rest of the principles so always keept it in mind.

### When NOT to Apply OCP
If a component handles 2-3 cases and is unlikely to ever grow, a simple if/else is clearer than an abstraction hierarchy. OCP is most valuable when the number of variants is open-ended or growing. 
Do not preemptively create extension points for cases that may never arrive. Refactor toward OCP when the third case appears, not when the first is written.


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


## DIP (Dependency Inversion Principle)
The Dependency Inversion Principle states that dependencies should **always** flow torward abstractions. If your components always depend on abstractions, they will automatically comply with the above principles (SRP, OCP, LSP). If your components depend on concrete implementations, you will have to change once they change as you are coupled with the concreteness.

Simple illustration:
```
+----------------+          +-------------+
|  Coffee Making |          | Water       |
|      Logic     |  ---->   | Compartment |
+----------------+          +-------------+
        |                   +-------------+
        └--------------->   | Coffee      |
                            | Compartment |
                            +-------------+
```

Here we have a component whose logic for a coffee maker does the following:
1. Heats up water to a certain temperature
2. Pressurizes it over ground coffee beans

It must have access to the component responsible for the water (to be able to heat it up) and to the component responsible for the ground coffee. It does not matter _how_ these components are implemented, it just needs access to specific features of each component (e.g: heating up the water). If the component responsible for the water had an abstraction, the logic component would never change.

If, one day, the water compartment's design changes (a different type of straw is used, for example) it will have an impact on the `Coffee Making Logic` component. So instead, we make sure each component is dependent on _abstractions_, or in other words, we _invert_ the dependencies:
```
+----------------+        +-------------------+          +-------------+
|  Coffee Making | -----> | Water Compartment |   <----- | Water       |
|      Logic     |        |       Interface   |          | Compartment |
+----------------+        +-------------------+          +-------------+
        |                 +---------------------+        +-------------+
        └-------------->  | Coffee Compartment  | <----- | Coffee      |
                          |       Interface     |        | Compartment |
                          +---------------------+        +-------------+
```
Now each component is dependent on _abstractions only_. This makes our code open for extentions (we can add new types of water compartments) without changing existing concrete compartment implementations.


## ADP (Acyclic Dependencies Principle)
The Acyclic Dependencies Principle is very upfront: There should be no cyclic dependencies between your components.
If cyclic dependencies exist, or without clear seperation of dependencies, introducing change can cause effects that are impossible to foresee or fix. There isn't alot to add here: **make sure you understand the direction of dependencies your components have**.

## SDP (Stable Dependencies Principle)
The Stable Dependencies Principle states that dependencies between components should **always** flow in the direction of the stability of the components. In other words, a component should only depend upon components that are more stable than it is.

### What is stability?
`Stable` means _"hard to change"_, whereas `unstable` means _"easy to change"_. _"hard to change"_ means that introducing a change to the component propogates change to any components who are directly or indirectly dependent upon the component.

A component becomes **more** stable the more it gets dependent upon, and the **less** it depends upon others.
An instable component is the oposite - one which depends upon others more then others depend upon it.

### Introducing Change to Stable Components
When change is introduced to the most stable components, the entire system is distrupted and requires change. Due to this you **always** want your component to be dependent upon _more_ stable components than it is. If your component depends upon _less_ stable components than it is, or in other words, components that change often, your entire system will constantly change.

If we guarantee that stability flows from highest to lowest in our dependency graph, we can introduce change to the least stable components without causing distruptions to other components.

### Examples

#### a
```python
def log(text: str):
    print(text)


class Worker
    def work():
        ...
        log(text="Did work")


def main():
    log(text="Starting worker")
    Worker().work()
    log(text="Worker finished")
```

As you can see, the `log()` function is used in several contexts in the above code example. It is very _dependent_ upon, meaning it is _stable_. If we change the `log()` function, by adding another print, then both `main()` and `Worker` will experience change. If we now introduce a dependency to `log()`, one which is very _unstable_ and experiences frequent change (for example: writing to a file - the name can change frequently depending on whos maintaining the code), it will cause all the dependent components to change frequently too. It would be smarter for

## SAP (Stable Abstractions Principle)