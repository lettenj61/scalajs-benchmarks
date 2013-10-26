# Scala.js Benchmarks

This is a port of the Dart
[benchmark harness](https://github.com/dartlang/benchmark_harness) to
[Scala.js](https://github.com/lampepfl/scala-js).

You can see the benchmarks in action
[here](http://jonas.github.io/scalajs-benchmarks/).

All derivative work is the copyright of their respective authors and
distributed under their original license. All original work unless otherwise
stated is distributed under the [same license as
Scala.js](https://github.com/jonas/scalajs-benchmarks/LICENSE).

## Get started

To run the benchmarks, first install the Scala.js compiler by following the
instructions in the [Scala.js README](https://github.com/lampepfl/scala-js).

Next, open `sbt` either in the root folder or in the folder of the benchmark
you want to run. Issue `packageJS` to compile the Scala code to JavaScript.
For benchmarks with a web version, open the `index-dev.html` of the in your
favorite Web browser. For benchmarks without a web version execute the
`run.sh` script inside the benchmark folder.

During development, it is useful to use `~packageJS` in sbt, so that each
time you save a source file, a compilation of the project is triggered.
Hence only a refresh of your Web page is needed to see the effects of your
changes.

## The optimized version

Instead of running `packageJS`, you can also run `optimizeJS` to generate
a much more compact version of the JavaScript code. While `index-dev.html`
refers to the JavaScript emitted by `packageJS`, `index.html` refers to the
optimized JavaScript emitted by `optimizeJS`.

The optimization phase is performed by the Advanced Optimizations of the
[Google Closure Compiler](https://developers.google.com/closure/compiler/),
which make
[strong assumptions](https://developers.google.com/closure/compiler/docs/api-tutorial3)
about the code being compiled.

All the code generated by Scala.js respects these assumptions. But if you
modify `exports.js`, make sure that you comply with them if you want to be
able to use `optimizeJS`.

## Troubleshooting

### I have unresolved dependencies on `scalajs-...`

You have probably forgotten to execute `publishLocal` from the sbt prompt in
Scala.js.

### WARNING - dangerous use of the global this object

This warning is emitted by the Google Closure Compiler when running
`optimizeJS`. The global `this` object is used in the `exports.js`
script as an alternative to `window` to export symbols used by the
various startup code.  This allows the benchmarks to run both in the web
browser and in the `d8` (the V8 shell), which does not define global
objects like `window`.
