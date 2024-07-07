# Asynchronous Programming

Both JavaScript and C support asynchronous programming models, which look similar to each other with respect to their usage. The following example shows, on a very high level, how async code looks like in JavaScript:

```js
async function printDelayed(message, cancellationToken) {
    await new Promise(resolve => setTimeout(resolve, 1000));
    return `Message: ${message}`;
}
```

C code is structured differently:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h> // Requires GCC
#include <unistd.h>

void* printDelayed(void* args) {
    char* message = (char*)args;
    sleep(1); // Simulating a delay of 1 second
    char* result = malloc(strlen(message) + 10); // Allocating memory for the result
    sprintf(result, "Message: %s", message);
    return result;
}

int main() {
    char* message = "Hello, World!";
    pthread_t tid;
    void* result;

    pthread_create(&tid, NULL, printDelayed, (void*)message);
    pthread_join(tid, &result);

    printf("%s\n", (char*)result);
    free(result); // Freeing allocated memory
    return 0;
}
```

See also:

- [Asynchronous programming in C]

[Asynchronous programming in C]: https://hackaday.com/2019/09/24/asynchronous-routines-for-c/

## Executing tasks

From the following example the `PrintDelayed` method executes, even though it is not awaited:

```js
let cancellationToken = undefined; 
printDelayed("message", cancellationToken); // Prints "message" after a second.
await new Promise(resolve => setTimeout(resolve, 2000));

async function printDelayed(message, cancellationToken) {
    await new Promise(resolve => setTimeout(resolve, 1000));
    console.log(message);
}
```

In C:

```c
#include <stdio.h>

#ifdef _WIN32
#include <windows.h>
#define sleep(x) Sleep(x * 1000)
#else
#include <unistd.h>
#endif

void printDelayed(char* message) {
    sleep(1); // Delay for 1 second
    printf("%s\n", message);
}

int main() {
    char* message = "message";
    printDelayed(message); // Prints "message" after a second.
    sleep(2); // Delay for 2 seconds
    return 0;
}
```

## Task cancellation

The previous JavaScript examples included passing a `CancellationToken` to asynchronous methods, as is considered best practice in JavaScript. `CancellationToken`s can be used to abort an asynchronous operation.

In C language, the concept ofcancellation  can be implemented using custom structures and functions.

## Executing multiple Tasks

In JavaScript, `Promise.race` is frequently used to handle the execution of multiple tasks.

`Promise.race` completes as soon as any task completes. 

```js
const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));

const delayMessage = async (delayTime) => {
    await delay(delayTime);
    return `Waited ${delayTime / 1000} second(s).`;
};

const delay1 = delayMessage(1000);
const delay2 = delayMessage(2000);

Promise.race([delay1, delay2]).then(result => {
    console.log(result); // Output: Waited 1 second(s).
});
```

In C, to achieve similar functionality to Promise.race in JavaScript, one can use a combination of threads and synchronization mechanisms like mutexes or semaphores. By creating multiple threads that perform tasks concurrently and using synchronization to determine which thread finishes first, a similar behavior to Promise.race can be achieved in C. Below is a simplified example in C:

```c
#include <stdio.h>
#include <pthread.h> // Requires GCC
#include <unistd.h>

void* delayMessage(void* delayTime) {
    int ms = *((int*)delayTime);
    usleep(ms * 1000); // Convert milliseconds to microseconds
    char* message = malloc(100 * sizeof(char));
    sprintf(message, "Waited %d second(s).", ms / 1000);
    return message;
}

int main() {
    pthread_t thread1, thread2;
    int delay1 = 1000;
    int delay2 = 2000;
    char* result1;
    char* result2;

    pthread_create(&thread1, NULL, delayMessage, (void*)&delay1);
    pthread_create(&thread2, NULL, delayMessage, (void*)&delay2);

    pthread_join(thread1, (void**)&result1);
    pthread_join(thread2, (void**)&result2);

    printf("%s\n", result1); // Output: Waited 1 second(s).

    free(result1);
    free(result2);

    return 0;
}
```

## Multiple consumers

In JavaScript a `Promise` can be used across multiple consumers. All of them can await the task and get notified when the task is completed or failed. 

In C:

```c
#include <stdio.h>
#include <stdlib.h>

#ifdef _WIN32
#include <windows.h> // For Sleep(), DWORD
#else
#include <unistd.h> // For sleep()
#include <sys/types.h> // For pid_t
#include <sys/wait.h> // For wait()
#endif

void background_operation() {
#ifdef _WIN32
    Sleep(2000);
#else
    sleep(2);
#endif
    printf("Background operation completed.\n");
}

#ifdef _WIN32
BOOL WINAPI signal_handler(DWORD dwCtrlType) {
    if (dwCtrlType == CTRL_C_EVENT) {
        printf("Signal received. Cancelling background operation.\n");
        exit(0);
    }
    return TRUE;
}
#else
void signal_handler(int signal) {
    if (signal == SIGINT) {
        printf("Signal received. Cancelling background operation.\n");
        exit(0);
    }
}
#endif

int main() {
#ifdef _WIN32
    SetConsoleCtrlHandler(signal_handler, TRUE);
#else
    signal(SIGINT, signal_handler);
#endif

    DWORD pid = GetCurrentProcessId(); // Get current process ID
    printf("Process ID: %lu\n", pid);

    // Fork and perform background operation
    // ...

    wait(NULL);
    printf("Parent process waiting for child process to finish.\n");

    return 0;
}
```

## Asynchronous iteration

C does not yet have an API for asynchronous iteration in the standard library.

In JavaScript, writing async iterators has comparable syntax to when writing synchronous iterators:

```js
async function* RangeAsync(start, count) {
    for (let i = 0; i < count; i++) {
        await new Promise(resolve => setTimeout(resolve, i * 1000));
        yield start + i;
    }
}

(async () => {
    for await (const item of RangeAsync(10, 3)) {
        console.log(item + " "); // Prints "10 11 12".
    }
})();
```

In C:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

typedef struct {
    int start;
    int count;
} Range;

void RangeAsync(Range range) {
    for (int i = 0; i < range.count; i++) {
        usleep(i * 1000000); // Simulating async delay in microseconds
        printf("%d ", range.start + i);
    }
}

int main() {
    Range range = {10, 3};
    RangeAsync(range);
    return 0;
}
```