# Producer-Consumer

The producer-consumer pattern is very common to distribute work between threads where data is passed from producing threads to consuming threads without the need for sharing or locking. 

```js
const workerCode = `
    self.onmessage = function() {
        const messages = [];
        for (let n = 1; n < 10; n++) {
            messages.push("Message #" + n);
        }
        self.postMessage(messages);
    };
`;

const blob = new Blob([workerCode], { type: "application/javascript" });
const worker = new Worker(URL.createObjectURL(blob));

// The main thread acts as a consumer here
worker.onmessage = function(event) {
    const messages = event.data;
    messages.forEach(message => console.log(message));
};

// Start the worker
worker.postMessage(null);

```

A rough translation of the above JavaScript example in C would look as follows:

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

#define BUFFER_SIZE 10

void* producer(void* arg) {
    for (int n = 1; n < 10; n++) {
        printf("Message #%d\n", n);
    }
    pthread_exit(NULL);
}

int main() {
    pthread_t tid;
    pthread_create(&tid, NULL, producer, NULL);

    // main thread acts as the consumer
    pthread_join(tid, NULL);

    return 0;
}
```
