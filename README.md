# Simulation-Guided Logic Synthesis and Verification
 Benchmarks and pattern files used in the simulation-guided Boolean resubstitution paper[1] and the simulation-guided LS&V paper[2].
 
 [1] Simulation-Guided Boolean Resubstitution. Siang-Yun Lee, Heinz Riener, Alan Mishchenko, Robert K. Brayton, Giovanni De Micheli. IWLS 2020. https://arxiv.org/abs/2007.02579
 
 [2] A Simulation-Guided Logic Synthesis and Verification Paradigm. (Under review)

## benchmarks
The original benchmarks are from [IWLS 2005](http://iwls.org/iwls2005/benchmarks.html) and pre-processed with [ABC](https://github.com/berkeley-abc/abc). The optimized benchmarks can be found here in `benchmarks_ifraig-s/`, which are preprocessed with `abc> ifraig -s` repeated until no more size reduction can be observed. 

A set of highly optimized benchmarks used for experiments on equivalence checking are in `benchmarks_dc2syn2/`, which are optimized first with `ifraig -s` until saturation and then three times of `&dc2; &syn2`.

## pattern files
A relatively expressive set of simulation patterns can be found in `256sa1obs_bitpacked/`, which were generated with 256 random patterns + 1x stuck-at-check + observability awareness. Generated patterns (i.e., except the 256 random ones) are bitpacked. They were used as the initial pattern set in resubstitution and appended with counter-examples. The final (appended) pattern sets can be found in `cex_basic/` (for the basic resubstitution setting of N=1, K=10) and `cex_extreme/` (for the extreme resubstitution setting of N=20, K=100) and are used in the quality experiments in the paper.

Note that five of the benchmarks are too large and the second case of observability check (please refer to the paper) is too slow, so only the first case was checked. They are: `leon2`, `leon3`, `leon3mp`, `leon3_opt` and `netcard`.

Also note that the pattern files of the five large benchmarks exceed the size limitation of GitHub. Hence, they are uploaded here: [256sa1obs_bitpacked](https://drive.google.com/drive/folders/1Ws9nBxLNNg74Nss9P5lq4RxUiWCtJedT?usp=sharing), [cex_basic](https://drive.google.com/drive/folders/1oM_FI3KQBYEU_2GPvEjtqX4_8U_oNp-o?usp=sharing), [cex_extreme](https://drive.google.com/drive/folders/1qtWNsEOWVNQcdcoisNcynZ9Ee7pKmYIb?usp=sharing).

## ECO benchmarks
A set of ECO benchmarks from [3] are used to verify the reusability of the patterns. They are translated from the original Verilog format into AIG format in `ECO_old/` and `ECO_new/`. Their generated patterns are also in `256sa1obs_bitpacked/`, which were generated with 256 random patterns + 1x stuck-at-check + observability awareness (only case 1).

[3] Learning to Automate the Design Updates From Observed Engineering Changes in the Chip Development Cycle. Victor N. Kravets, Jie-Hong R. Jiang, Heinz Riener. DATE 2020.


## Experiment settings
The algorithms are implemented in [mockturtle](https://github.com/lsils/mockturtle). Please refer to its [documentation](https://mockturtle.readthedocs.io/en/latest/installation.html) for instructions on the installation of the library.

Note that many changes have been made in mockturtle since the implementation of this work, thus different results may be achieved. In order to obtain similar results, please clone from pull request [#396](https://github.com/lsils/mockturtle/pull/396) or commit [ba2ce8d](https://github.com/lsils/mockturtle/commit/ba2ce8d3b0a89d9096404f7c861e3103038e9d95).

Also note that although the IWLS'05 benchmarks come with mockturtle (in `mockturtle/experiments/benchmarks`), they should be replaced with the preprocessed ones in this repository in order to obtain the same results as in the paper. The pattern files should be placed in the `build` path.

### pattern generation
The simulation patterns in `256sa1obs_bitpacked` were obtained by running [`pattern_generation`](https://github.com/lsils/mockturtle/blob/master/experiments/pattern_generation.cpp) with the following configuration:

`num_random_pattern = 256;`  
`ps.odc_levels = 5;`  

Note that the obtained patterns may still be different because of the different implementation of random generators on different machines.

### simulation-guided resubstitution
The resubstitution quality comparison results were obtained by running different configurations of [`sim_resubstitution`](https://github.com/lsils/mockturtle/blob/master/experiments/sim_resubstitution.cpp).

The baseline resubstitution settings with N=1, K=10 or 100 (Table IV, middle columns) are configured as follows:

`//ps.pattern_filename = "256sa1obs_bitpacked/" + benchmark + ".pat"; //first time`  
`ps.pattern_filename = "cex_basic/" + benchmark + ".pat"; //second time`  
`ps.save_patterns = "cex_basic/" + benchmark + ".pat";`  
`ps.max_pis = 10;`  
`ps.max_divisors = 150;`  
`ps.max_inserts = 1;` 

or

`ps.pattern_filename = "cex_basic/" + benchmark + ".pat";`  
`ps.save_patterns = "cex_basic/" + benchmark + ".pat";`  
`ps.max_pis = 100;`  
`ps.max_divisors = 2000;`  
`ps.max_inserts = 1;` 

The best-achievable resubstitution settings with N=20, K=100 (Table IV, last column) are configured as follows:

`//ps.pattern_filename = "cex_basic/" + benchmark + ".pat"; //first time`  
`ps.pattern_filename = "cex_extreme/" + benchmark + ".pat"; //second time`  
`ps.save_patterns = "cex_extreme/" + benchmark + ".pat";`  
`ps.max_pis = 100;`  
`ps.max_divisors = 2000;`  
`ps.max_inserts = 20;` 

### simulation-guided combinational equivalence checking
The experiments of CEC were done in [ABC](https://github.com/berkeley-abc/abc) with the following commands:
`&read benchmarks_ifraig-s/$benchmarkname.aig; &sim_read 256sa1obs_bitpacked/$benchmarkname.pat; &cec benchmarks_dc2syn2/$benchmarkname.aig`
