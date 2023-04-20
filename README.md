# ReScript v10 vs v11 Benchmark Test

## Summary

**TL;DR: So far, there are observed changes, the impact in the real world is negligibly small.**

### Main difference(s) between versions:

- Variant tags is representing in plain string instead of integer. This may affect performance and bundle size when defaulted.

The major change is the change to the default variant representation. If that affects you, you should customize it via the `@as` tag.

See the [diff file](upgrade.diff) for more detail.

## Performance

Tested `node benchmark.js` in Node.js v18.14.0, Apple M1 Pro

```
decode10 x 445,681 ops/sec ±0.42% (97 runs sampled)
decode11 x 438,521 ops/sec ±0.48% (97 runs sampled)
```

About 1% ~ 2% perf drop by the string tags.

## Bundle Size

Bundled by esbuild v0.17.17

|        | size             | size (min)       | size (min+gzip) |
| -------| ---------------- | ---------------- | --------------- |
| /w v10 | 17,877 (17.9 kB) | 6,028 (6 kB)     | 2,081 (2.1 kB)  |
| /w v11 | 17,538 (17.5 kB) | 6,524 (6.5 kB)   | 2,129 (2.1 kB)  |

Bundle size may increase as it uses variant matches.

However, repetitive string tags are effectively compressed by gzip/brotli, so they have less of an impact in the real world.
