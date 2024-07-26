# Copying Operation

The following code will copy `test.txt` to `test2.txt`.

In C:

```js
#include <stdio.h>

int main() {
    FILE *source, *target;
    char ch;

    source = fopen("test.txt", "r");
    if (source == NULL) {
        printf("Error opening source file.\n");
        return 1;
    }

    target = fopen("test2.txt", "w");
    if (target == NULL) {
        fclose(source);
        printf("Error opening target file.\n");
        return 1;
    }

    while ((ch = fgetc(source)) != EOF) {
        fputc(ch, target);
    }

    printf("File copied successfully.\n");

    fclose(source);
    fclose(target);

    return 0;
}
```

In Node.js:

```js
const fs = require('fs');

fs.copyFile('test.txt', 'test2.txt', (err) => {
  if (err) throw err;
  console.log('test.txt was copied to test2.txt');
});
```

In Deno:

```js
// to run the program, execute `deno run --allow-all copying-operations.js`
const sourceFile = await Deno.open('test.txt');
const destinationFile = await Deno.create('test2.txt');

await Deno.copy(sourceFile, destinationFile);

sourceFile.close();
destinationFile.close();

console.log('File copied successfully!');
```