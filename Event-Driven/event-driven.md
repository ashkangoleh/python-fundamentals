# Event-Driven


`Event-driven` programming is a programming paradigm in which the flow of the program is determined by events such as user actions, sensor inputs, or messages from other programs. In Python, event-driven programming can be implemented using libraries such as asyncio, Twisted, or Tornado.

One of the key benefits of event-driven programming is that it allows for `highly concurrent` and `scalable applications`. By `using non-blocking I/O` and `asynchronous` programming techniques, event-driven applications can handle `large numbers of events` and requests `without blocking` or `slowing down the program`.


#### Here's an example of event-driven programming in Python using asyncio:

```python
import asyncio

async def handle_event(event):
    print(f"Received event: {event}")

async def main():
    # Set up event loop
    loop = asyncio.get_event_loop()

    # Register event handlers
    loop.create_task(handle_event("stdin"))
    loop.create_task(asyncio.sleep(5))
    loop.create_task(handle_event("timer"))

    # Run event loop
    await asyncio.sleep(10)

if __name__ == "__main__":
    asyncio.run(main())
```

The code sets up an event loop using `asyncio.get_event_loop()`. It then creates three tasks using `loop.create_task()` and adds them to the event loop.
The first task is created using `loop.create_task(handle_event("stdin"))`. This creates a task for the handle_event coroutine with the argument "stdin". The handle_event coroutine simply prints the event it receives.

The second task is created using `loop.create_task(asyncio.sleep(5))`. This creates a task that sleeps for 5 seconds. This ensures that the event loop continues running for at least 5 seconds before the next coroutine is executed.

The third task is created using `loop.create_task(handle_event("timer"))`. This creates a task for the `handle_event` coroutine with the argument "timer". The `handle_event` coroutine simply prints the event it receives.

After creating the tasks, the code calls await `asyncio.sleep(10)` to keep the event loop running for 10 seconds. This allows the tasks to be executed when the appropriate events occur.

When the event loop runs, it will execute the tasks in the order they were created. In this case, it will execute the `handle_event("stdin")` task first, which will print "Received event: stdin". It will then execute the `asyncio.sleep(5)` task, which will sleep for 5 seconds. After 5 seconds have elapsed, it will execute the `handle_event("timer")` task, which will print "Received event: timer".
Overall, the code sets up a simple event loop that creates three tasks and prints the events they receive. The `asyncio.sleep()` task is used to delay the execution of the third task by 5 seconds. This ensures that the tasks are executed in the correct order.



## [Twisted](https://twisted.org/) -  An event-driven networking engine

Event-driven programming in Python using the Twisted library:

```python
from twisted.internet import protocol, reactor, endpoints

class Echo(protocol.Protocol):
    def dataReceived(self, data):
        self.transport.write(data)

class EchoFactory(protocol.Factory):
    def buildProtocol(self, addr):
        return Echo()

endpoints.serverFromString(reactor, "tcp:8000").listen(EchoFactory())
reactor.run()
```

In this example, we define a simple TCP echo server using the Twisted library. We define a `Protocol` subclass `Echo` that simply echoes back any data it receives, and a `Factory` subclass `EchoFactory` that creates instances of the `Echo` protocol. We then use the `reactor` object to listen on port 8000 and run the event loop.

When you run this code, it will start a TCP server that listens on port 8000. 
Any data sent to the server will be echoed back to the client.

This example demonstrates how event-driven programming can be used to build network servers that handle multiple connections concurrently. By using non-blocking I/O and asynchronous programming techniques, the server can handle many simultaneous connections without blocking or slowing down the program.

## AsyncIO

`Asyncio` is a library in Python that provides infrastructure for writing single-threaded concurrent code using coroutines, multiplexing I/O access over sockets and other resources, running network clients and servers, and other related primitives. It is designed to be fast and efficient, and is particularly well-suited for network programming and other I/O-bound tasks.

One of the key benefits of asyncio is that it allows for highly concurrent and scalable applications. By using non-blocking I/O and asynchronous programming techniques, asyncio applications can handle large numbers of events and requests without blocking or slowing down the program.


Here's an example of asyncio in Python:

```python
import asyncio

async def my_coroutine():
    print("Coroutine started")
    await asyncio.sleep(1)
    print("Coroutine finished")

async def main():
    print("Main started")
    await asyncio.gather(my_coroutine(), my_coroutine())
    print("Main finished")

if __name__ == "__main__":
    asyncio.run(main())
```
In this example, we define a coroutine `my_coroutine` that simply prints out a message, waits for 1 second using `asyncio.sleep`, and then prints out another message. We then define the `main` coroutine, which prints out a message, runs two instances of `my_coroutine` concurrently using `asyncio.gather`, and then prints out another message. Finally, we run the event loop using `asyncio.run(main())`.

When you run this code, it will start the event loop and run the main coroutine. The main coroutine will start two instances of my_coroutine concurrently, which will print out their messages and then wait for 1 second. After 1 second, the coroutines will finish and the main coroutine will print out its final message.
This example demonstrates how asyncio can be used to write concurrent and scalable code in Python. By using coroutines and the asyncio library, you can write code that can handle many simultaneous events and requests without blocking or slowing down the program.