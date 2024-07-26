# Binary File Operations

In the following example, the binary data of the ASCII character `a` will be written to `test.bin`, and the content of `test.bin` will be read and displayed on the console. The size of the binary file is determined at runtime.

In C:

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *file;
    char data = 'a';

    // Writing binary data to test.bin
    file = fopen("test.bin", "wb");
    fwrite(&data, sizeof(char), 1, file);
    fclose(file);

    // Reading and displaying binary data from test.bin
    file = fopen("test.bin", "rb");
    fseek(file, 0, SEEK_END);
    long fileSize = ftell(file);
    rewind(file);

    char *buffer = (char *)malloc(fileSize);
    fread(buffer, sizeof(char), fileSize, file);
    fclose(file);

    for (int i = 0; i < fileSize; i++) {
        printf("%c", buffer[i]);
    }

    free(buffer);
    return 0;
}
```

In Node.js:

```js
const fs = require('fs');

const data = Buffer.from('a', 'ascii');

fs.writeFile('test.bin', data, (err) => {
  if (err) throw err;

  fs.readFile('test.bin', (err, data) => {
    if (err) throw err;
    console.log(data.toString('ascii'));
  });
});
```

In Deno:

```js
// to run the program, execute `deno run --allow-all binary-operations.js`
const encoder = new TextEncoder();
const data = encoder.encode('a');

await Deno.writeFile("test.bin", data);

const file = await Deno.open("test.bin");
const content = new Uint8Array(await Deno.readAll(file));
Deno.close(file.rid); // Close the file using the file descriptor

const decoder = new TextDecoder();
console.log(decoder.decode(content));
```