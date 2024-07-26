# Deleting Operations

The following code will delete `test.txt`.

In C:

```c
#include <stdio.h>

int main() {
    if (remove("test.txt") == 0) {
        printf("File deleted successfully.\n");
    } else {
        printf("Error deleting the file.\n");
    }
    return 0;
}
```

In Node.js:

```js
const fs = require('fs');

const filePath = 'test.txt';

fs.unlink(filePath, (err) => {
    if (err) {
        console.error(`Error deleting the file: ${err}`);
        return;
    }
    console.log('File deleted successfully');
});
```

In Deno:

```js
// to run the program, execute `deno run --allow-all deleting-operations.js`
try {
  Deno.removeSync("test.txt");
  console.log("File 'test.txt' has been deleted successfully.");
} catch (error) {
  console.error("An error occurred:", error);
}
```