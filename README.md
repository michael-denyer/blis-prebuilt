# blis-prebuilt

Pre-built [BLIS](https://github.com/flame/blis) shared libraries with **ILP64** (64-bit integer) BLAS interface for use with [jamma](https://github.com/michael-denyer/jamma).

## Build configuration

All builds use:
- `-b 64` — BLAS/CBLAS interface uses 64-bit integers (exports `dgemm64_`)
- `-i 64` — internal BLIS integer size is 64-bit
- `--enable-cblas` — CBLAS wrapper enabled

## Platforms

| Asset | OS | Architecture | BLIS config |
|-------|-----|-------------|-------------|
| `libblis-x86_64.so` | Linux | x86_64 | haswell |
| `libblis-firestorm.dylib` | macOS | Apple Silicon | firestorm |
| `libblis-haswell.dylib` | macOS | Intel | haswell |

## Usage

These libraries are downloaded automatically by jamma's `hatch_build.py` during wheel builds. The `blas_dispatch.c` runtime discovers the bundled library via `dlopen` and resolves the `dgemm64_` symbol for ILP64 dispatch.

## Why ILP64 only?

LP64 (32-bit integer) BLAS uses different FP accumulation order than jamma's own GEMM implementation, causing numerical inconsistencies with GEMMA reference results. ILP64 is required for both correctness at scale (>46k samples) and numerical consistency.
