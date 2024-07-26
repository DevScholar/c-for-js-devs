# Moving Operations

The following code will move `test.txt` to `./test_folder/test.txt`.

In C:

```c
#include <stdio.h>

int main() {
    if(rename("test.txt", "./test_folder/test.txt") == 0) {
        printf("File moved successfully.\n");
    } else {
        perror("Error moving file");
    }
    return 0;
}
```

In Node.js:

```js
const fs = require('fs');

const sourcePath = 'test.txt';
const destinationPath = './test_folder/test.txt';

fs.rename(sourcePath, destinationPath, (err) => {
    if (err) {
        console.error(`Error moving the file: ${err}`);
    } else {
        console.log('File moved successfully!');
    }
});
```

In Deno:

```js
// to run the program, execute `deno run --allow-all moving-operations.js`
const oldFilePath = 'test.txt';
const newFilePath = './test_folder/test.txt';

await Deno.rename(oldFilePath, newFilePath);
console.log(`File moved successfully from ${oldFilePath} to ${newFilePath}`);
```