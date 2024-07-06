# Synchronization

When data is shared between threads, one needs to synchronize read-write access to the data in order to avoid corruption. In JavaScript:

```js
let data = 0;
let workers = [];
let completedWorkers = 0;

for (let i = 0; i < 10; i++) {
    let worker = new Worker('data:text/javascript,' + encodeURIComponent(`
        let partialData = 0;
        for (let j = 0; j < 1000; j++) {
            partialData++;
        }
        self.postMessage(partialData);
    `));
    
    worker.onmessage = function(event) {
        data += event.data;
        completedWorkers++;
        if (completedWorkers === 10) {
            console.log(data);
            workers.forEach(function(worker) {
                worker.terminate();
            });
        }
    };
    
    workers.push(worker);
    worker.postMessage(null);
}

```
Using threading and synchronization mechanisms available in C:

```c
#include <stdio.h>
#include <pthread.h>

#define NUM_THREADS 10
#define NUM_INCREMENTS 1000

int data = 0;
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

void* thread_function(void* arg) {
    for (int i = 0; i < NUM_INCREMENTS; i++) {
        pthread_mutex_lock(&mutex);
        data++;
        pthread_mutex_unlock(&mutex);
    }
    pthread_exit(NULL);
}

int main() {
    pthread_t threads[NUM_THREADS];

    for (int i = 0; i < NUM_THREADS; i++) {
        pthread_create(&threads[i], NULL, thread_function, NULL);
    }

    for (int i = 0; i < NUM_THREADS; i++) {
        pthread_join(threads[i], NULL);
    }

    printf("%d\n", data);

    return 0;
}
```
