# Design Patterns


Design patterns are `reusable solutions` to common software design problems. They provide a way to solve problems that arise during software development in a `consistent and efficient` manner. Design patterns are not specific to a particular programming language or technology, but rather are `general solutions` that can be applied to a wide range of software development problems.

Design patterns are typically classified into three categories: `creational`, `structural`, and `behavioral`. Creational patterns deal with `object creation mechanisms`, trying to create objects in a manner suitable to the situation. Structural patterns deal with `object composition`, trying to form large structures from individual objects. Behavioral patterns deal with `communication between objects`, trying to define the ways objects interact and communicate with each other.

Design patterns are often used to improve the `quality`, `maintainability`, and `scalability` of software systems. They can help to `reduce code complexity`, improve code reuse, and make software systems more `flexible and adaptable` to changing requirements. By using design patterns, developers can build software systems that are `easier to understand`, `modify`, and `maintain` over time.
Some of the most commonly used design patterns include the `Singleton`, `Factory`, `Observer`, `Decorator`, `Adapter`, `Strategy`, `Command`, `Template Method`, `Facade`, and `Iterator` patterns. Each pattern has its own `strengths and weaknesses`, and should be chosen based on the specific needs of the system being developed.


1. `Singleton`: This pattern ensures that a class has only one instance, and provides a global point of access to that instance. This is useful for managing resources that should only exist in a single instance, such as a database connection or a configuration object.

    ```python
    class SingletonMeta:
        _instances = {}

        def __call__(cls, *args, **kwargs):
            if cls not in cls._instances:
                cls._instances[cls] = super().__call__(*args, **kwargs)
            return cls._instances[cls]
            
    class Singleton(metaclass=SingletonMeta):
        def some_business_logic(self):
            """
            Finally, any singleton should define some business logic, which can be
            executed on its instance.
            """
            ...
    if __name__ == "__main__":
        # The client code.

        s1 = Singleton()
        s2 = Singleton()

        if id(s1) == id(s2):
            print("Singleton works, both variables contain the same instance.")
        else:
            print("Singleton failed, variables contain different instances.")
    ```
    ``` Output> "Singleton works, both variables contain the same instance."```

    Now creating `Thread-Safe`:
    ```python
    from threading import Lock, Thread


    class SingletonMeta(type):
        """
        This is a thread-safe implementation of Singleton.
        """

        _instances = {}

        _lock: Lock = Lock()
        """
        We now have a lock object that will be used to synchronize threads during
        first access to the Singleton.
        """

        def __call__(cls, *args, **kwargs):
            """
            Possible changes to the value of the `__init__` argument do not affect
            the returned instance.
            """
            # Now, imagine that the program has just been launched. Since there's no
            # Singleton instance yet, multiple threads can simultaneously pass the
            # previous conditional and reach this point almost at the same time. The
            # first of them will acquire lock and will proceed further, while the
            # rest will wait here.
            with cls._lock:
                # The first thread to acquire the lock, reaches this conditional,
                # goes inside and creates the Singleton instance. Once it leaves the
                # lock block, a thread that might have been waiting for the lock
                # release may then enter this section. But since the Singleton field
                # is already initialized, the thread won't create a new object.
                if cls not in cls._instances:
                    instance = super().__call__(*args, **kwargs)
                    cls._instances[cls] = instance
            return cls._instances[cls]


    class Singleton(metaclass=SingletonMeta):
        value: str = None
        """
        We'll use this property to prove that our Singleton really works.
        """

        def __init__(self, value: str) -> None:
            self.value = value

        def some_business_logic(self):
            """
            Finally, any singleton should define some business logic, which can be
            executed on its instance.
            """


    def test_singleton(value: str) -> None:
        singleton = Singleton(value)
        print(singleton.value)


    if __name__ == "__main__":
        # The client code.

        print("If you see the same value, then singleton was reused (yay!)\n"
            "If you see different values, "
            "then 2 singletons were created (booo!!)\n\n"
            "RESULT:\n")

        process1 = Thread(target=test_singleton, args=("FOO",))
        process2 = Thread(target=test_singleton, args=("BAR",))
        process1.start()
        process2.start()
    ```

2. `Factory`: This pattern provides an interface for creating objects, but allows subclasses to decide which class to instantiate. This is useful for creating objects that require complex initialization or configuration, or for creating objects that belong to a family of related classes.

    ```python
    class User:
        def __init__(self, name):
            self.name = name

    class Admin(User):
        def __init__(self, name):
            super().__init__(name)
            self.role = "admin"

    class Guest(User):
        def __init__(self, name):
            super().__init__(name)
            self.role = "guest"

    class UserFactory:
        def create_user(self, name, role):
            if role == "admin":
                return Admin(name)
            elif role == "guest":
                return Guest(name)
            else:
                raise ValueError("Invalid role")
    ```

    In this example, we have a main `User` class that has a `name` attribute. We then create two subclasses, `Admin` and `Guest`, both of which inherit from the `User` class and have a `role` attribute. We then create a `UserFactory` class that has a `create_user` method that creates an appropriate user object for each type of user.
    Using the Factory pattern, we can easily create an appropriate user object for each user without having to repeat code. For example, to create a user object with the name "John" and the role "admin", we can use the following code:

    ```python
    factory = UserFactory()
    user = factory.create_user("John", "admin")
    print(user.name)  # Output: John
    print(user.role)  # Output: admin
    ```
    In this example, we create a `UserFactory` object and then use its `create_user` method to create a user object with the name "John" and the role "admin". We then print the `name` and `role` attributes of this object.

3. `Observer(proxy)`: This pattern defines a one-to-many dependency between objects, so that when one object changes state, all its dependents are notified and updated automatically. This is useful for building event-driven systems, where objects need to respond to changes in other objects.

    ```python
    class Subject:
        def __init__(self):
            self._observers = []

        def attach(self, observer):
            self._observers.append(observer)

        def detach(self, observer):
            self._observers.remove(observer)

        def notify(self):
            for observer in self._observers:
                observer.update()

    class Observer:
        def update(self):
            pass

    class ConcreteObserver(Observer):
        def update(self):
            print("Observer notified")

    if __name__ == "__main__":
        subject = Subject()
        observer = ConcreteObserver()
        subject.attach(observer)
        subject.notify()
    ```

    In this example, we define a `Subject` class that maintains a list of observers and provides methods for attaching, detaching, and notifying observers. We also define an `Observer` base class and a `ConcreteObserver` subclass that overrides the update method to print a message when notified. Finally, we create a `Subject` instance, attach a `ConcreteObserver` instance, and notify the observer.
