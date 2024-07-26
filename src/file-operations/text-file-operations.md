# Text File Operations

Write and read a text file in C:

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *file;
    char text[] = "sample text";
    
    // Write to file
    file = fopen("test.txt", "w");
    if (file == NULL) {
        printf("Error opening file!\n");
        exit(1);
    }
    fprintf(file, "%s", text);
    fclose(file);
    
    // Read from file
    char buffer[255];
    file = fopen("test.txt", "r");
    if (file == NULL) {
        printf("Error opening file!\n");
        exit(1);
    }
    fscanf(file, "%s", buffer);
    printf("Content of test.txt: %s\n", buffer);
    fclose(file);
    
    return 0;
}
```

In Node.js:

```js
const fs = require('fs');

// Write to file
fs.writeFileSync('test.txt', 'sample text');

// Read from file
const data = fs.readFileSync('test.txt', 'utf8');
console.log(data);
```

In Deno:

```js
// to run the program, execute `deno run --allow-all text-operations.js`
const text = "sample text";
const encoder = new TextEncoder();
const data = encoder.encode(text);

await Deno.writeFile("test.txt", data);

const file = await Deno.open("test.txt");
const fileContent = new Uint8Array(100);
await Deno.read(file.rid, fileContent);

const decoder = new TextDecoder();
const decodedText = decoder.decode(fileContent);

console.log(decodedText);

Deno.close(file.rid);
```