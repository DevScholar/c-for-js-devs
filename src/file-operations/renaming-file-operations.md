# Renaming Operations

The following code will rename `old_name.txt` to `new_name.txt`.

In C:

```js
#include <stdio.h>

int main() {
    const char *old_name = "old_name.txt";
    const char *new_name = "new_name.txt";

    if (rename(old_name, new_name) == 0) {
        printf("File renamed successfully.\n");
    } else {
        perror("Error renaming file");
    }

    return 0;
}
```

In Node.js:

```js
const fs = require('fs');

const oldPath = 'old_name.txt';
const newPath = 'new_name.txt';

fs.rename(oldPath, newPath, (err) => {
    if (err) {
        console.error(err);
        return;
    }
    console.log('File renamed successfully!');
});
```

In Deno:

```js
// to run the program, execute `deno run --allow-all renaming-operations.js`

// Define the old and new file names
const oldName = "old_name.txt";
const newName = "new_name.txt";

// Rename the file synchronously
Deno.renameSync(oldName, newName);

console.log(`File ${oldName} has been renamed to ${newName} successfully.`);
```