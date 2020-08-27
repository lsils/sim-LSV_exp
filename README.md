# sim_resub_exp
 Benchmarks and pattern files used in the simulation-guided Boolean resubstitution paper[1]
 
 [1] Simulation-Guided Boolean Resubstitution. Siang-Yun Lee, Heinz Riener, Alan Mishchenko, Robert K. Brayton, Giovanni De Micheli. IWLS 2020. https://arxiv.org/abs/2007.02579

## benchmarks
The original benchmarks are from [IWLS 2005](http://iwls.org/iwls2005/benchmarks.html), and pre-processed with `ifraig` command in [ABC](https://github.com/berkeley-abc/abc), repeated until no more size reduction can be observed. The optimized benchmarks can be found here in `sim_resub_exp/benchmarks/`. The benchmarks in `sim_resub_exp/benchmarks_2/` are pre-processed with `ifraig -s`.

## pattern files
A relatively expressive set of simulation patterns can be found in `sim_resub_exp/256sa1obs_bitpacked/`, which was generated with 256 random patterns + 1x stuck-at-check + observability awareness. Generated patterns (i.e., except the 256 random ones) are bitpacked. They were used as the initial pattern set in resubstitution and appended with counter-examples. The final (appended) pattern sets can be found in `sim_resub_exp/cex` and are used in the quality experiments in the paper.

Note that five of the benchmarks are too large and the second case of observability check (please refer to the paper) is too slow, so only the first case was checked. They are: `leon2`, `leon3`, `leon3mp`, `leon3_opt` and `netcard`.

Also note that the pattern files of the five large benchmarks exceed the size limitation of GitHub. Hence, they are uploaded here: [256sa1obs_bitpacked](https://drive.google.com/drive/folders/1Ws9nBxLNNg74Nss9P5lq4RxUiWCtJedT?usp=sharing), [cex](https://drive.google.com/drive/folders/1zuLa7A0-ywNWSTtYww-orClAV06fjYVP?usp=sharing).
