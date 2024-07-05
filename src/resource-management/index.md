# Resource Management

Previous section on [memory management] explains the differences between JavaScript and C when it comes to GC. It is highly recommended to read it.

This section is limited to providing an example of a fictional _database connection_ involving a SQL connection to be properly closed/disposed/dropped.

```js
class DatabaseConnection {
    constructor(connectionString) {
        this.connectionString = connectionString;
    }

    closeConnection() {
        // Implementation to close the connection
    }
}

// ...closing connection...
DatabaseConnection.prototype.close = function() {
    this.closeConnection();
    console.log(`Closing connection: ${this.connectionString}`);
};

// Create instances of DatabaseConnection
const db1 = new DatabaseConnection("Server=A;Database=DB1");
const db2 = new DatabaseConnection("Server=A;Database=DB2");

// ...code for making use of the database connection...
// "Dispose" of "db1" and "db2" when their scope ends
```

```c
#include <stdio.h>
#include <stdlib.h>

// Define a structure to represent DatabaseConnection
typedef struct {
    char* connectionString;
} DatabaseConnection;

// Function to create a new DatabaseConnection instance
DatabaseConnection* createDatabaseConnection(const char* connectionString) {
    DatabaseConnection* db = (DatabaseConnection*)malloc(sizeof(DatabaseConnection));
    db->connectionString = strdup(connectionString);
    return db;
}

// Function to close the connection
void closeConnection(DatabaseConnection* db) {
    // Implementation to close the connection
    printf("Closing connection: %s\n", db->connectionString);
    free(db->connectionString);
    free(db);
}

int main() {
    // Create instances of DatabaseConnection
    DatabaseConnection* db1 = createDatabaseConnection("Server=A;Database=DB1");
    DatabaseConnection* db2 = createDatabaseConnection("Server=A;Database=DB2");

    // ...code for making use of the database connection...

    // "Dispose" of "db1" and "db2" when their scope ends
    closeConnection(db1);
    closeConnection(db2);

    return 0;
}
```

[memory management]: ../memory-management/index.md
