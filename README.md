# sim_resub_exp
 Benchmarks and pattern files used in the simulation-guided Boolean resubstitution paper[1]
 
 [1] Simulation-Guided Boolean Resubstitution. Siang-Yun Lee, Heinz Riener, Alan Mishchenko, Robert K. Brayton, Giovanni De Micheli. IWLS 2020. https://arxiv.org/abs/2007.02579

## benchmarks
The original benchmarks are from [IWLS 2005](http://iwls.org/iwls2005/benchmarks.html), and pre-processed with `ifraig` command in [ABC](https://github.com/berkeley-abc/abc), repeated until no more size reduction can be observed. The optimized benchmarks can be found here in `sim_resub_exp/benchmarks/`.

## pattern files
A relatively expressive set of simulation patterns can be found in `sim_resub_exp/256sa1obs/`, which was generated with 256 random patterns + 1x stuck-at-check + observability awareness. They were used as the initial pattern set (appended with counter-examples) in the quality experiments in the paper.

Note that five of the benchmarks are too large such that their pattern files exceed the size limitation of GitHub. Hence, they are uploaded [here](https://drive.google.com/drive/folders/1DILPi8NVwsYkFjuKWwQVKg4zmWBDC6Ie?usp=sharing).
