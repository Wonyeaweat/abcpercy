License: MIT - https://opensource.org/licenses/MIT

# Efficient Multi-Output LUT Mapping

This library provides efficient multi-output LUT mapping code based on ABC FPGA mapping.

Code location:
zinterface/abcorig/src

All modified sections in the source code are marked with the keyword:
user define

----------------------------------------
## How to build

Prerequisites:
- A working C/C++ toolchain (gcc/clang, make)
- CMake (version 3.x)
- Any dependencies required by ABC (install system packages as needed for your platform)

Quick build (from repository root):

```bash
mkdir -p build
cd build
cmake ..
cmake --build . -j$(nproc)
```

This will build the project and produce the `abc` executable under the build tree, for example:

```
build/zinterface/abcorig/abc
```

----------------------------------------
## run

Common Commands:
xxx.v; if -K 6; write_blif xxx.blif

- Input: xxx.v (Verilog file)
- Command: if -K 6 (LUT mapping with K=6)
- Output: xxx.blif (BLIF format file)


Quick run examples:

run a sequence of abc commands (read Verilog, map with K=6, write BLIF)

```
build/zinterface/abcorig/abc -c "read zepfl/adder.v; if -K 6; write_blif zepfl/adder_mapped_k6.blif; quit"
```

The example above reads the Verilog `zepfl/adder.v`, performs FPGA LUT mapping with K=6, and writes the mapped BLIF to `zepfl/adder_mapped_k6.blif`.

----------------------------------------
## Testcases

Example testcases and small benchmarks are stored in the `zepfl/` directory. 


----------------------------------------
## Output Format

The output file format is consistent with the native ABC output.
All lines that are merged into the same cut are marked with one of the following:

1. Co-root Example:
   CRX2645647
   "CR" = co-root

2. CRsingle:
   Used when the number of inputs is less than 3,
   and can be merged with other similar types.

----------------------------------------





