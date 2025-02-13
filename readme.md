# `crc.crc32`

A CRC32 checksum for python - implemented in Nim lang for lower-level language.

Usage:

```py
from crc import crc32

crc32('Hello World')
0x4a17b156
```


+ **Is fast fast fast**
    Using a compiled nim module under-the-hood we gain c-speed fastness. Pre-computing the CRC table ensures only XOR bit manipulation is the main task.
+ **Table-Driven Approach:**
    Precomputing a lookup table transforms an algorithm that could have been computationally expensive into one that’s highly efficient.
+ **Made in Nim (exported to Python)**
    Nim's `nimpy` module makes it simple to expose native Nim code to Python.
+ Matches online examples:
    Using polynomial `0xEDB88320` ensures we match JS and other online examples.


## Benchmark

> Benchmarks mean nothing. 99% of the time they only detail the 1% of perfect cases. That said - Checkout _this_ benchmark:


+ Best result operations per second is 1% faster than `anycrc` (the fastest) - especially from cold-start.
+ On average it's \~5% slower (AFTER WARMUP!)
+ But also; `any_crc` is 4% slower than its own fastest run

```
Name (time in ns)                                        OPS (Kops/s)            Rounds  Iterations
---------------------------------------------------------------------------------------------------
test_anycrc_32 (NOW)                                         834.4711 (1.0)       98107           1
test_anycrc_32 (windows-cpython-3.8-64bit/0030_51bb0b1)      802.7725 (0.96)      85845           1
test_crc_32 (NOW)                                            796.8278 (0.95)      78040           1
test_crc_32 (windows-cpython-3.8-64bit/0030_51bb0b1)         730.7685 (0.88)      98107           1
```

Caveats:

+ speed runs are from cold-start, then 4 hot runs. `any crc` is faster after warm-up.
+ This is an older version of Nim, Windows, and Python - newer will probably be faster
+ *This is using a custom compiled python 3.8 - for other reasons...*

Legend:

+ `test_anycrc_32`: the `anycrc` standard 32 poly model.
+ `test_crc_32`: my implementation - same poly.

> `1.0` means _the target - as the fastest_
+ `0.99` defines _1% from the target fastest_.


```

Name                     Outliers  OPS (Kops/s)            Rounds
-------------------------------------------------------------------
test_anycrc_32          5906;5906      755.0418 (0.99)      79854
test_crc_32             2497;2497      759.6637 (1.0)       92799
-------------------------------------------------------------------



Name                       Min                    Max                  Mean
--------------------------------------------------------------------------------------
test_anycrc_32        873.0000 (1.0)      80,088.0000 (1.0)      1,324.4300 (1.01)
test_crc_32           873.0000 (1.00)     95,522.0000 (1.19)     1,316.3719 (1.0)
--------------------------------------------------------------------------------------


Name                   StdDev                Median                 IQR
--------------------------------------------------------------------------------------
test_anycrc_32       676.4560 (1.11)     1,165.0000 (1.0)      291.0000 (1.00)
test_crc_32          611.1147 (1.0)      1,165.0000 (1.0)      291.0000 (1.0)
--------------------------------------------------------------------------------------


Legend:
  Outliers: 1 Standard Deviation from Mean; 1.5 IQR (InterQuartile Range) from 1st Quartile and 3rd Quartile.
  OPS: Operations Per Second, computed as 1 / Mean
```

General benchmarks:

```
Name (time in ns)              IQR             Outliers  OPS (Kops/s)            Rounds  Iterations
-----------------------------------------------------------------------------------------------------------------------------------
test_anycrc_32 (stored)   291.0000 (>1000.0)  6130;6130      864.9554 (1.0)       85845           1
test_anycrc_32 (NOW)      291.0000 (>1000.0)  1410;2646      859.3654 (0.99)      81760           1
test_crc_32 (stored)      291.0000 (>1000.0)  1786;3553      716.0017 (0.83)      83753           1
test_crc_32 (NOW)           0.0000 (1.0)     1507;23624      790.0587 (0.91)      90359           1
-----------------------------------------------------------------------------------------------------------------------------------
```


### Compatability

The results are a HEX (octet) numbers and uses the standard crc polynomial. Therefore results are compatible with [online examples](https://stackoverflow.com/questions/18638900/javascript-crc32)

Python:

```py
# python
from crc import crc32
python_result = crc32('The quick brown fox jumps over the lazy dog')
0x414FA339
```

JS:

```js
// js
let msg = 'The quick brown fox jumps over the lazy dog'
// An exact value from py
python_result = 0x414FA339
// https://www.npmjs.com/package/@tsxper/crc32
const result = crc32(msg)
1095738169 // is 0x414FA339 in integer form.
result == python_result

crc32(msg).toString(16)
'414fa339'
```

---

