# Threading

The C standard library supports threading, synchronisation and concurrency. Also the language itself and the standard library do have basic support for the concepts, a lot of additional functionality is provided by third party libraries and will not be covered in this document.

For threading in C, the pthread library is commonly used:

To compile the code, use the -pthread flag with gcc:
`gcc -pthread main.c -o main`


```c
#include <stdio.h>
#include <pthread.h>

void *thread_function(void *arg) {
    // Thread logic here
    return NULL;
}

int main() {
    pthread_t thread_id;
    pthread_create(&thread_id, NULL, thread_function, NULL);
    
    // Main program logic here
    
    pthread_join(thread_id, NULL);
    
    return 0;
}
```

For synchronization and concurrency in C, mechanisms like mutexes and semaphores can be utilized. Here is a basic example using a mutex for synchronization:

```c
#include <stdio.h>
#include <pthread.h>

pthread_mutex_t mutex;

void *thread_function(void *arg) {
    pthread_mutex_lock(&mutex);
    
    // Critical section
    
    pthread_mutex_unlock(&mutex);
    return NULL;
}

int main() {
    pthread_t thread_id;
    pthread_mutex_init(&mutex, NULL);
    
    pthread_create(&thread_id, NULL, thread_function, NULL);
    
    // Main program logic here
    
    pthread_join(thread_id, NULL);
    
    pthread_mutex_destroy(&mutex);
    
    return 0;
}
```
To compile the code, use the -pthread flag with gcc:
`gcc -pthread main.c -o main`

JavaScript is a single-threaded scripting language that does not support multithreading.

<!--The following lists approximate mapping of threading types and methods in .NET
to Rust:

| .NET               | Rust                      |
| ------------------ | ------------------------- |
| `Thread`           | `std::thread::thread`     |
| `Thread.Start`     | `std::thread::spawn`      |
| `Thread.Join`      | `std::thread::JoinHandle` |
| `Thread.Sleep`     | `std::thread::sleep`      |
| `ThreadPool`       | -                         |
| `Mutex`            | `std::sync::Mutex`        |
| `Semaphore`        | -                         |
| `Monitor`          | `std::sync::Mutex`        |
| `ReaderWriterLock` | `std::sync::RwLock`       |
| `AutoResetEvent`   | `std::sync::Condvar`      |
| `ManualResetEvent` | `std::sync::Condvar`      |
| `Barrier`          | `std::sync::Barrier`      |
| `CountdownEvent`   | `std::sync::Barrier`      |
| `Interlocked`      | `std::sync::atomic`       |
| `Volatile`         | `std::sync::atomic`       |
| `ThreadLocal`      | `std::thread_local`       |
-->
Below is a simple JavaScript program that creates a thread (where the thread prints some text to standard output) indirectly by using `worker` and then waits for it to end:

```js
// Equivalent JavaScript code using Web Workers
const worker = new Worker(URL.createObjectURL(new Blob([`
    self.onmessage = function(event) {
        console.log(event.data);
    };
`], { type: 'application/javascript' })));

worker.postMessage('Hello from a thread!');
```

The same code in C would be as follows:

```c
#include <stdio.h>
#include <pthread.h>

void *thread_function(void *arg) {
    printf("Hello from a thread!\n");
    return NULL;
}

int main() {
    pthread_t thread;
    pthread_create(&thread, NULL, thread_function, NULL);
    pthread_join(thread, NULL); // wait for thread to finish
    return 0;
}
```

In JavaScript, it's possible to send data as an argument to a thread:
```js
const workerCode = `
self.onmessage = function(e) {
    let eventData = e.data;
    eventData += (" World!");
    console.log("Phrase: " + eventData);
};
`;

const blob = new Blob([workerCode], { type: "application/javascript" });
const worker = new Worker(URL.createObjectURL(blob));

const data = "Hello";
worker.postMessage(data);
```

<!--However, a more modern or terser version would use closures:

```csharp
using System;
using System.Text;
using System.Threading;

var data = new StringBuilder("Hello");

var t = new Thread(obj => data.Append(" World!"));

t.Start();
t.Join();

Console.WriteLine($"Phrase: {data}");
```-->

In C, the data is passed to the thread:

```c
#include <stdio.h>
#include <pthread.h>
#include <string.h>

void* thread_function(void* arg) {
    char* data = (char*)arg;
    strcat(data, " World!");
    return data;
}

int main() {
    char data[20] = "Hello";
    pthread_t thread;
    pthread_create(&thread, NULL, thread_function, (void*)data);
    void* result;
    pthread_join(thread, &result);
    printf("Phrase: %s\n", (char*)result);
    return 0;
}
```
