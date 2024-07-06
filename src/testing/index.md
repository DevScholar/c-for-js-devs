# Testing

## Test organization

JavaScript solutions use separate projects to host test code, irrespective of the test framework being used (Jest, Mocha, etc.) and the type of tests (unit or integration) being written. The test code therefore lives in a separate assembly than the application or library code being tested. In C, developers use a third party tool called CUnit for testing.


See also:

- [Test Organization][test-org]

  [test-org]: https://cunit.sourceforge.net/doc/managing_tests.html

## Running tests

CUnit supports running all tests in all registered suites, but individual tests or suites can also be run. During each run, the framework keeps track of the number of suites, tests, and assertions run, passed, and failed. Note that the results are cleared each time a test run is initiated (even if it fails).

For more information, see "[Running Tests in CUnit][tests-exec]".

  [tests-exec]: https://cunit.sourceforge.net/doc/running_tests.html

## Output in Tests

For very complex integration or end-to-end test, developers sometimes log what's happening during a test. The actual way they do this varies with each test framework in C.

## Assertions
JavaScript users have multiple ways to assert, depending on the framework being used. For example, an assertion in Jest might look like:

```js
test('something has the right length', () => {
    let value = "something";
    expect(value.length).toBe(9);
});
```

An example that only uses vanilla JavaScript:

```js
function somethingIsTheRightLength() {
    let value = "something";
    console.assert(value.length === 9);
}

somethingIsTheRightLength();
```

C does not require a separate framework like CUnit for asserting.

Below is an example:

```c
#include <assert.h>
#include <string.h>

int main() {
    void test_something_is_the_right_length() {
        const char *value = "something";
        assert(strlen(value) == 9);
    }

    test_something_is_the_right_length(); // Call the test function
    return 0;
}
```

The standard library does not offer anything in the direction of data-driven tests.

## Mocking

There are libraries for C too, like [`umock_c`][umock_c], that can help with mocking. However, it is also possible to use [conditional compilation].

The example below shows mocking of a stand-alone function `var_os` and returns the value of an environment variable. It conditionally imports a mocked version of the `var_os` function:

```c
#include <stdio.h>

#ifdef TEST
    #define var_os var_os_mock
    char* var_os_mock() {
        return "Mocked Value";
    }
#else
    char* var_os() {
        return "Actual Value";
    }
#endif

void get_env() {
    char* value = var_os();
    printf("Environment Variable Value: %s\n", value);
}

int main() {
    #ifndef TEST
        get_env();
    #endif

    return 0;
}
```

  [umock_c]: https://github.com/Azure/umock-c
  [conditional compilation]: ../conditional-compilation/index.md

## Code coverage

There is sophisticated tooling for JavaScript when it comes to analyzing test code coverage, such as Jest.

In C, one can implement code coverage functionalities by utilizing tools like gcov or LLVM's coverage tools. These tools can help collect test code coverage data in C programs. By incorporating these tools into the C codebase, developers can achieve code coverage analysis.