# Qfplib-M3: 

## A free, fast and accurate ARM Cortex-M3 floating-point library



## Introduction

Qfplib-M3 is a library of IEEE 754 single-precision floating-point arithmetic routines for microcontrollers based on the ARM Cortex-M3 core (ARMv7-M architecture). It will also run on Cortex-M4 microcontrollers but is not optimised for these devices. The optimisation goals for Qfplib-M3 are speed and accuracy, while keeping code size within reasonable bounds.

Qfplib-M3 provides correctly rounded (to nearest, even-on-tie) addition, subtraction, multiplication, division and square root operations, and sine, cosine, tangent, arctangent, logarithm and exponential functions that give a very high degree of accuracy.

## Licence

Qfplib-M3 is open source, licensed under version 2 of the [GNU GPL](http://www.gnu.org/licenses/). Use at your own risk. Qfplib-M3 is **not** licensed under the LGPL. Roughly speaking, this means that if you wish to use it in conjunction with non-GPL code you will require an alternative licence: please enquire using the e-mail address on the [home page](https://www.quinapalus.com/index.html).

## Code size

The complete set of functions in Qfplib-M3 occupies a little under 12 kbyte of program memory. In general code is not shared between the functions, so the footprint can be reduced significantly if not all functions are used: if using the GNU linker, supply the `--gc-sections` option. Qfplib-M3 does not depend on any other libraries.

## Stack and static memory usage

Qfplib-M3 uses no stack and no static storage. No initialisation is required. The code is fully ROMable and is thread-safe.

## Speed and accuracy

The following table compares cycle counts for Qfplib-M3 against other libraries. Qfplib-M3 timing results are approximate average values over the ranges of argument values shown and include a calling overhead of 3 cycles. They were measured using an LPC1763 microcontroller executing from (single-cycle) RAM.

In the following table, ‘ulp’ means ‘unit in last place’. Errors are measured relative to the correctly rounded result.

| Function          | Arguments                         | **Qfplib-M3** | ‘GoFast’                | IAR    | Keil                   |        |          |      |      |
| ----------------- | --------------------------------- | ------------- | ----------------------- | ------ | ---------------------- | ------ | -------- | ---- | ---- |
| **Cycles**        | **Accuracy**                      | Cycles        | Accuracy                | Cycles | Accuracy               | Cycles | Accuracy |      |      |
| `qfp_fadd(x,y)`   | 2–16≤\|*x*\|<216 2–16≤\|*y*\|<216 | **37.1**      | **Exact in all cases**  | 90     | ±1 ulp ‘in most cases’ | 60     | ?        | 55   | ?    |
| `qfp_fsub(x,y)`   | 2–16≤\|*x*\|<216 2–16≤\|*y*\|<216 | **38.0**      | 95                      | 60     | 55                     |        |          |      |      |
| `qfp_fmul(x,y)`   | 2–16≤\|*x*\|<216 2–16≤\|*y*\|<216 | **36.0**      | 80                      | 50     | 50                     |        |          |      |      |
| `qfp_fdiv(x,y)`   | 2–16≤\|*x*\|<216 2–16≤\|*y*\|<216 | **57.1**      | 195                     | 80     | 135                    |        |          |      |      |
| `qfp_fsqrt(x)`    | 2–16≤*x*<216                      | **49.3**      | 380                     | 565    | 260                    |        |          |      |      |
| `qfp_fexp(x)`     | 2–4≤\|*x*\|<24                    | **44.1**      | **±1 ulp in all cases** | 210    | ±2 ulp ‘in most cases’ | 1635   | ?        | 1565 | ?    |
| `qfp_fln(x)`      | 2–16≤*x*<216                      | **44.4**      | 455                     | 830    | 825                    |        |          |      |      |
| `qfp_fsin(x)`     | 2–8≤\|*x*\|<1                     | **43.0**      | 205                     | 750    | 710                    |        |          |      |      |
| 2–8≤\|*x*\|<28    | **60.1**                          |               |                         |        |                        |        |          |      |      |
| All *x*           | **63.9**                          |               |                         |        |                        |        |          |      |      |
| `qfp_fcos(x)`     | 2–8≤\|*x*\|<1                     | **39.2**      | 205                     | 740    | 705                    |        |          |      |      |
| 2–8≤\|*x*\|<28    | **59.4**                          |               |                         |        |                        |        |          |      |      |
| All *x*           | **65.1**                          |               |                         |        |                        |        |          |      |      |
| `qfp_ftan(x)`     | 2–8≤\|*x*\|<1                     | **48.2**      | 345                     | 825    | 835                    |        |          |      |      |
| 2–8≤\|*x*\|<28    | **70.5**                          |               |                         |        |                        |        |          |      |      |
| All *x*           | **72.5**                          |               |                         |        |                        |        |          |      |      |
| `qfp_fatan2(y,x)` | 2–4≤\|*x*\|<24 2–4≤\|*y*\|<24     | **83.4**      | 540                     | 860    | 965                    |        |          |      |      |

Note that unlike Qfplib-M3, none of the alternative libraries appears to offer IEEE 754 compliance with regard to rounding.

Results for the Micro Digital ‘GoFast’, Keil and IAR libraries are inferred from the timings given [here](http://www.smxrtos.com/ussw/gofast/gofast_thumb2_keil.htm) and [here](http://www.smxrtos.com/ussw/gofast/gofast_thumb2_iar.htm). Those pages have not been updated for a few years: I would welcome any more up-to-date benchmark figures. Note however, that (for example) the [end-user licence for the Keil MDK](http://www.keil.com/Content/eula/mdkeula.html) includes the clause ‘you shall treat any and all benchmarking data relating to the Software [...] which are indicative of its performance, efficacy, reliability or quality, as confidential information and you shall not disclose such information to any third party without the express written permission of ARM’. It is not clear whether such a clause is enforceable, but it nevertheless could be viewed as an indication of ARM’s confidence in the ‘performance, efficacy, reliability or quality’ of their software.

## Accuracy analysis of scientific functions

| Function     | Mean signed (systematic) error | Mean unsigned error | RMS error  | Worst-case negative error | Worst-case positive error |
| ------------ | ------------------------------ | ------------------- | ---------- | ------------------------- | ------------------------- |
| `qfp_fexp`   | +0.0036 ulp                    | 0.0216 ulp          | 0.1471 ulp | –1 ulp                    | +1 ulp                    |
| `qfp_fln`    | –0.0413 ulp                    | 0.0417 ulp          | 0.2042 ulp | –1 ulp                    | +1 ulp                    |
| `qfp_fsin`   | –0.0019 ulp                    | 0.0115 ulp          | 0.1074 ulp | –1 ulp                    | +1 ulp                    |
| `qfp_fcos`   | –0.0011 ulp                    | 0.0119 ulp          | 0.1092 ulp | –1 ulp                    | +1 ulp                    |
| `qfp_ftan`   | –0.0247 ulp                    | 0.0561 ulp          | 0.2368 ulp | –1 ulp                    | +1 ulp                    |
| `qfp_fatan2` | +0.0144 ulp                    | 0.0186 ulp          | 0.1364 ulp | –1 ulp                    | +1 ulp                    |

Care has been taken to ensure Qfplib-M3 maintains results accurate to 1 ulp in pathological cases, such as sin *x* where *x* is near a multiple of π, and cos *x* and tan *x* where *x* is near an odd multiple of π/2. It even correctly evaluates sin(16367173·273). I would be interested to learn of any applications that require such accuracy other than the testing of floating-point libraries or the evaluation of π to many digits, noble pursuits though those both are.

## Testing

Each unary function has been tested against the standard GNU floating-point library supplied with GCC for x86 processors exhaustively on non-exceptional arguments, plus on tens of millions of random exceptional cases. In exceptional cases the unary functions return bit-identical results to the GNU library; `qfp_fsqrt` returns bit-identical results in all cases.

Each binary function has been tested against the GNU x86 library on over a billion cases, exceptional and non-exceptional, random and contrived. For `qfp_fatan2` bit-identical results are returned in all exceptional cases; for all other binary functions, bit-identical results are returned in all cases.

## Implementation of the IEEE 754 standard

Qfplib correctly treats signed zeros, denormals, infinities and NaNs according to the IEEE 754 standard. The results of the addition, subtraction, multiplication, division and square root operations are correctly rounded (to nearest, even-on-tie). Other rounding modes and traps are not supported.

## Other functions

You may also be interested in the `qfp_fcmp`, `qfp_float2int`, `qfp_float2fix`, `qfp_int2float`, `qfp_fix2float`, `qfp_float2uint`, `qfp_float2ufix`, `qfp_uint2float`, `qfp_ufix2float`, `qfp_float2str` and `qfp_str2float` functions provided as part of [Qfplib-M0-tiny](https://github.com/mysterywolf/Qfplib-M0-tiny) library.

## Files

- qfplib-m3.s, the source code to Qfplib-M3. The GNU assembler syntax is used.

- qfplib-m3.h, a C header file giving prototypes for the Qfplib-M3 functions.

Visit http://www.quinapalus.com/qfplib-m3.html for more information.
