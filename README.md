# SSE4-CRC32

Starting with the Nehalam series, Intel processors feature the Streaming SIMD Extensions instruction set which
provide a hardware-accelerated version of the CRC32 algorithm (Castagnoli variant). This library uses the
Intel SSE 4.2 instruction set to provide a fast CRC-32 algorithm.


## Features

- Intel Streaming SIMD Extensions 4.2 based hardware accelerated CRC-32C calculation
- Graceful fallback to software-based CRC (table-based CRC calculation)
- Progressive CRC-32C calculation
- Supports Node.js buffers


## Performance

The tests were run on a Macbook Air running an Intel Core i7 processor, with 8GB of RAM and used buffers
instead of strings to prevent having items on the V8 heap that might cause the garbage collector to fire
frequently and interfere with the test run-times.

Below are the results from the 2 test cases:

    > node benchmark/1.single_1kb_length_buffer.benchmark.js
    100000 calls to calculate CRC on a 1024 byte buffer...
        Hardware-based CRC-32C: 27.388368 ms
        Software-based CRC-32C: 87.832587 ms
        Pure JS CRC-32C (table-based): 726.95698 ms
        Pure JS CRC-32C (direct): 4332.993692 ms

    > node benchmark/2.multi_random_length_buffer.benchmark.js
    100000 calls to calculate CRC on random-length buffers upto 4096 bytes long...
        Avg. buffer length: 2049 bytes
        Hardware-based CRC-32C: 88.440507 ms
        Software-based CRC-32C: 173.136638 ms
        Pure JS CRC-32C (table-based): 2131.861339 ms
        Pure JS CRC-32C (direct): 9006.901272 ms


## Installation

Use the following command to install the library from npm:

    npm install sse4_crc32


## Usage

Using the library is quite simple. Start out by including the library in your code:

    var Sse4Crc32 = require("sse4_crc32");

To calculate the 32-bit CRC for any string, simply use the following code:

    var crc = Sse4Crc32.calculate("my string");

Instead of passing in a string, a buffer can be passed to the `calculate()` function.

Furthermore, the `calculate()` function takes an optional `initialCrc` value as the second argument, allowing
for progressive CRC calculation.

    var crc = Sse4Crc32.calculate("my string");
    var newCrc = Sse4Crc32.calculate("my new string", crc);

Another way to calculate CRC in a progressive manner is as follows:

    var sse4crc32 = new Sse4Crc32.CRC32();
    var myInputs = [ "some string", new Buffer("a buffer"), "yet another string", new Buffer("yet another buffer") ];
    var crc;

    myInputs.forEach(function (string) {
        sse4crc32.update(string);
    });

    crc = sse4crc32.crc();

Also see the example code in the [examples](https://github.com/anandsuresh/sse4_crc32/tree/master/examples)
directory.

## How to compile

Once the repository has been cloned, use one of the following commands to build the library:

    make all            // Builds the release version of the library and runs all tests
    make debug          // Builds the debug version of the library
    make clean          // Removes all files generated by builds


## Contact

All feedback/suggestions/criticisms can be directed to [Anand Suresh](http://www.github.com/anandsuresh)
