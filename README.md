## coreJSON Library

This repository contains the coreJSON library, a parser that strictly enforces the ECMA-404 JSON standard and is suitable for low memory footprint embedded devices. The coreJSON library is distributed under the [MIT Open Source License](LICENSE.md).

This library has gone through code quality checks including verification that no function has a [GNU Complexity](https://www.gnu.org/software/complexity/manual/complexity.html) score over 8, and checks against deviations from mandatory rules in the [MISRA coding standard](https://www.misra.org.uk/MISRAHome/MISRAC2012/tabid/196/Default.aspx). Deviations from the MISRA C:2012 guidelines are documented under [MISRA Deviations](MISRA.md). This library has also undergone both static code analysis from [Coverity static analysis](https://scan.coverity.com/), and validation of memory safety and proof of functional correctness through the [CBMC automated reasoning tool](https://www.cprover.org/cbmc/).  

See memory requirements for this library [here](https://docs.aws.amazon.com/embedded-csdk/202012.00/lib-ref/libraries/standard/coreJSON/docs/doxygen/output/html/index.html#json_memory_requirements).

## Reference example

```c
#include <stdio.h>
#include "core_json.h"

int main()
{
    // Variables used in this example.
    JSONStatus_t result;
    char buffer[] = "{\"foo\":\"abc\",\"bar\":{\"foo\":\"xyz\"}}";
    size_t bufferLength = sizeof( buffer ) - 1;
    char queryKey[] = "bar.foo";
    size_t queryKeyLength = sizeof( queryKey ) - 1;
    char * value;
    size_t valueLength;
    
    // Calling JSON_Validate() is not necessary if the document is guaranteed to be valid.
    result = JSON_Validate( buffer, bufferLength );
    
    if( result == JSONSuccess )
    {
        result = JSON_Search( buffer, bufferLength, queryKey, queryKeyLength,
                              &value, &valueLength );
    }
    
    if( result == JSONSuccess )
    {
        // The pointer "value" will point to a location in the "buffer".
        char save = value[ valueLength ];
        // After saving the character, set it to a null byte for printing.
        value[ valueLength ] = '\0';
        // "Found: bar.foo -> xyz" will be printed.
        printf( "Found: %s -> %s\n", queryKey, value );
        // Restore the original character.
        value[ valueLength ] = save;
    }

    return 0;
}
```
A search may descend through nested objects when the `queryKey` contains matching key strings joined by a separator, `.`. In the example above, `bar` has the value `{"foo":"xyz"}`. Therefore, a search for query key `bar.foo` would output `xyz`.

## Building coreJSON

A compiler that supports **C90 or later** such as *gcc* is required to build the library.

Additionally, the library uses 2 header files introduced in ISO C99, `stdbool.h` and `stdint.h`. For compilers that do not provide this header file, the [source/include](source/include) directory contains [stdbool.readme](source/include/stdbool.readme) and [stdbool.readme](source/include/stdint.readme), which can be renamed to `stdbool.h` and `stdint.h` respectively.

For instance, if the example above is copied to a file named `example.c`, *gcc* can be used like so:
```bash
gcc -I source/include example.c source/core_json.c -o example
./example
```

*gcc* can also produce an output file to be linked:
```bash
gcc -I source/include -c source/core_json.c
```

## Generating documentation

The Doxygen references were created using Doxygen version 1.8.20. To generate the
Doxygen pages, please run the following command from the root of this repository:

```shell
doxygen docs/doxygen/config.doxyfile
```

## Building unit tests

### Checkout Unity Submodule
By default, the submodules in this repository are configured with `update=none` in [.gitmodules](.gitmodules), to avoid increasing clone time and disk space usage of other repositories (like [amazon-freertos](https://github.com/aws/amazon-freertos) that submodules this repository).

To build unit tests, the submodule dependency of Unity is required. Use the following command to clone the submodule:
```
git submodule update --checkout --init --recursive --test/unit-test/Unity
```

### Platform Prerequisites

- For running unit tests
    - C90 compiler like gcc
    - CMake 3.13.0 or later
    - Ruby 2.0.0 or later is additionally required for the Unity test framework (that we use).
- For running the coverage target, gcov is additionally required.

### Steps to build Unit Tests

1. Go to the root directory of this repository. (Make sure that the **Unity** submodule is cloned as described [above](#checkout-unity-submodule).)

1. Create build directory: `mkdir build && cd build`

1. Run *cmake* while inside build directory: `cmake -S ../test`

1. Run this command to build the library and unit tests: `make all`

1. The generated test executables will be present in `build/bin/tests` folder.

1. Run `ctest` to execute all tests and view the test run summary.

## Contributing

See [CONTRIBUTING.md](./.github/CONTRIBUTING.md) for information on contributing.
