=========
Changelog
=========

Not released
------------

v0.6.4 (2025/06/24)
-------------------

* Fix build system (no more unwanted packages) and fix CI configuration (#203).

v0.6.3 (2025/05/06)
-------------------

* Fix LDA backend (``select_vars``) and improve numerical stability (#200).

v0.6.2 (2025/04/15)
-------------------

* Optimize a bit sparse generic factors in SASCA (#195).
* Add ``scalib.attacks.Cpa``: Correlation Power Analysis tool (#196).

v0.6.1 (2025/03/25)
-------------------

* Build with the NTL dependency enabled for the linux CI.
* Change CI build to require x86-64-v3 instead of just AVX2. This is roughly
  equivalent in terms of supported CPUs, but allows more instructions to be
  used.
* Support numpy 2.
* Raise minimum supported python version to 3.10.
* Added a new histogram-based ranking method called 'scaledhist'.
  This method enables correct FFT convolutions using scaled
  histograms and improves precision by incorporating upper and lower histograms (#184).
* In ``BPState``, add a `vars` parameter to ``propagate_factor`` and a
  ``factors`` pararameter to ``propagate_var`` (#190).
* In ``BPState``, deprecate ``set_distribution``, replace by ``drop_distribution`` (#190).
* Add ``LdaAcc`` and ``Lda`` classes: more efficient multi-variable LDA, with API separation between fit and prediction steps (#192).
* Deprecate ``LdaClassifier`` and ``MultiLda`` (#192)

v0.6.0 (2024/09/03)
-------------------

* **Breaking** change parameter names ``l`` to ``traces`` for many functions.
* **Breaking** remove unnecessary parameters from initializers (e.g. trace
  length where it can be inferred).
* Fix misc. SASCA bugs with PUB, additions, generic factors

v0.5.8 (2024/05/21)
-------------------

* Add ``scalib.preprocessing.Quantizer`` for conversions of floating-point
  traces to int.
* Support modular subtractions (generalized summation) in FactorGraph.
* Raise minimum supported python version to 3.9.

v0.5.7 (2024/03/18)
-------------------

* ``scalib.attacks.FactorGraph``:

    * Add ``GENERIC`` factors

    * Add clear beliefs flag for BP.

    * Remove divisions in belief propagation (improves numerical robustness, fixes some NaN issues).

* Support python 3.12 in releases.

v0.5.6 (2023/06/08)
-------------------

* Add ``scalib.modeling.RLDAClassifier`` and ``scalib.metrics.RLDAInformationEstimator``.
* Fix build dependences and run build in an isolated environment.

v0.5.5 (2023/05/30)
-------------------

* Fix ``Distribution`` size bug in belief propagation computation (#102).
* Do not include debug symbols in release builds (makes wheel much smaller).
* ``Ttest``: do not crash on non-contiguous/fortran-order arrays
* Improve examples and README

v0.5.4 (2023/04/26)
-------------------

* Run CI on Mac Os and build wheels (x86_64 and arm64, but we test only x86_64). No AVX2 due to old runner in github CI.
* Add ``scalib.tools.ContextExecutor``, as a solution to ``LookupError`` in
  ``get_config()``.
* Fix numerical underflow in ``BPState`` when multiple traces are used.
* Fix missing import in ``MultiLDA``.
* Run ``BPState`` methods on the threadpool.
* Make threadpool initalization lazy -- makes SCALib play more nicely with ``ProcessPoolExecutor``.

v0.5.3 (2023/02/21)
-------------------

* Add ``vars`` and ``factors`` methods on `FactorGraph`.
* Add ``fg`` property on `BPState`.
* Fix bug in belief propagation. Numerical instabilities could lead to negative
  probabilities for AND (and probably XOR and ADD) factors.

v0.5.2 (2023/02/17)
-------------------

* Restructure and expand generated docs.
* Refactor doc generation.
* Change docs style.

v0.5.1 (2023/02/16)
-------------------

* Fix bug in ``rank_estimation``.
* Improve documentation and README.
* Run rust tests.
* Better selection of default threadpool size.

v0.5.0 (2023/02/08)
-------------------

* Deprecate ``SASCAGraph`` in favor of the new ``FactorGraph`` which provides:

    - Overall much less restrictions on operations, including suport for:

        + non-bijective tables
        + multiple public operands
        + NOT gate

    - A ``sanity_check`` feature: verify that the graph is compatible with known value assignment (useful for debugging a graph).
    - Optimized bitwise AND: ``O(nc*log(nc))`` instead of ``O(nc^2)``.

* Re-design ``scalib.config`` to handle more configuration in a single ``Config`` class. **Breaking change** to ``scalib.config`` and ``scalib.config.threading``.
* Smarter behavior for progress bars, unified configuration for progress bar. **Breaking change** to ``scalib.attack.SASCAGraph``.
* Add accessors for the internal state of the LDA.
* Introduce the ``scalib.ScalibError`` exception and remove ``scalib.metrics.SnrError`` (**Breaking change**).
* Improve error reporting in case of LDA solving error.
* Allow all computations to be interrupted with Ctrl-C.
* Fix deadlock when there is an error in large SNR computations (i.e., when ``n_vars*n_samples*n_traces > 2**33``).
* Allow LDA to behave like simple pooled gaussian templates (#22).
* Refresh build system (Tox version 4, improved CI), build by default with native machine optimizations.
* Not crash anymore on non x86-64 CPUs (no CI for those yet).

v0.4.3 (2022/10/27)
-------------------

* Upgrade CI to 3.11.
* Update dependancies and add python 3.10 to CI (#49)

v0.4.2 (2022/05/31)
-------------------

* Fix AVX2 not used when building rust dependencies.

v0.4.1 (2022/05/31)
-------------------

* Fix docs not building

v0.4.0 (2022/05/31)
-------------------

* SASCA: support modular ADD and MUL operations (#18)
* TTest: Performance improvement by using a mix of 2 passes and 1 pass algorithms 
* MTTest: First implementation of multivariate T-test.
* Improved documentation and README.rst
* SNR: use pooled formulas for better correctness then there are few traces,
  saves RAM (up to 75% reduction) and improves perf (about 2x single-threaded).
* Bump python minimum version to 3.7
* Revamp multi-threading handling thanks to new `scalib.threading` module.
* AVX2: Wheels on PyPi are built with AVX2 feature. 

v0.3.4 (2021/12/27)
-------------------

* Release GC in SASCA's `run_bp` .
* Release GC in `rank_accurary` and `rank_nbin`.
* `LDA.predict_proba` is marked thread-safe.
* Hide by default the progress bar of `SASCAGraph.run_bp` (can be re-enable
  with the `progress` parameter).

v0.3.3 (2021/07/13)
-------------------

* Solving minor issues in `MultiLDA` and `LDAClassifier`. Allowing multiple
  threads in `predict_proba()` and add a `done` flag to `solve()`.

v0.3.2 (2021/07/12)
-------------------

* Chunk `SNR.fit_u` to maintain similar performances with long traces and
  adding a progress bar 

v0.3.1 (2021/06/03)
-------------------

* Add `max_nb_bin` parameter to `postprocessing.rank_accuracy` (that was
  previously hard-coded).

v0.3.0 (2021/06/01)
-------------------

* Rename `num_threads` parameter of `modeling.MultiLDA` to `num_cpus`.
* Fix rank estimation when there is only one key chunk.
* Improve performance of `SNR.get_snr`.

v0.2.0 (2021/05/20)
-------------------

* Remove OpenBLAS and LAPACK, use Spectra and nalgebra instead.
* Use BLIS for matrix multiplications (Linux-only for now).
* Make `modeling.LDAClassifier` incremental (breaking change).
* Add `modeling.MultiLDA`.

v0.1.1 (2021/04/26)
-------------------

* Fix "invalid instruction" bug for CI wheel on windows.

v0.1.0 (2021/04/16)
-------------------

* Initial release, with the following features:
  * LDA and Gaussian templates modeling
  * SNR
  * T-test any order (for TLVA)
  * Soft Analytical Side-Channel Attack (SASCA)
  * Rank Estimation
