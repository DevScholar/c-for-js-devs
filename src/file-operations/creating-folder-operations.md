# Folder Operations

The following code will create a folder called `test_folder`.

In C:

```c
#include <stdio.h>

#ifdef _WIN32
    #include <direct.h>
    #define CREATE_DIR(path) _mkdir(path)
#else
    #include <sys/stat.h>
    #define CREATE_DIR(path) mkdir(path, 0777)
#endif

int main() {
    char* folderName = "test_folder";

    if (CREATE_DIR(folderName) == 0) {
        printf("Folder created successfully.\n");
    } else {
        printf("Failed to create folder.\n");
    }

    return 0;
}
```

In Node.js:

```js
const fs = require('fs');

const folderName = 'test_folder';

fs.mkdir(folderName, (err) => {
    if (err) {
        console.error(err);
        return;
    }
    console.log(`Folder '${folderName}' created successfully.`);
});
```

In Deno:

```js
// to run the program, execute `deno run --allow-all folder-operations.js`
const { mkdir } = Deno;

const folderName = "test_folder";

try {
  await mkdir(folderName);
  console.log(`Folder '${folderName}' created successfully.`);
} catch (error) {
  console.error(`Error creating folder: ${error.message}`);
}
```
