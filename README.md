# PHP 7.2.34 Security Backport

An independently verified security backport for PHP 7.2.34, based on patches distributed by Ondrej Sury.

## Overview

This repository contains a unified patch affecting 17 files in the PHP 7.2.34 source tree.

The backport incorporates changes associated with:
- CVE-2026-109618
- CVE-2026-109619

The upstream fix is associated with php-src commit:
`3584a98bda0f2c0d367255b046798b0d3a221a19`

## Provenance and credits

This patch combines changes from the following Ondrej Sury PHP packaging patches:

- `0135-backport-ZSTR_MAX_LEN.patch`
- `0159-Avoid-state-corruption-via-bailout.patch`

Original patch authors:
- Remi Collet: `0135-backport-ZSTR_MAX_LEN.patch` (cherry-picked from commit `37056ad634d9c44bac0d6c8e730eafaec1344840`).
- Arnaud Le Blanc, co-authored by Ilija Tovilo: `0159-Avoid-state-corruption-via-bailout.patch`.

The patches were obtained from the PHP packaging maintained by Ondrej Sury. The security fixes are credited to their original authors.

This repository does not claim original authorship of those security fixes.

## Applying the patch

Download the official PHP 7.2.34 source archive and extract it.

From the extracted source directory, run:

```sh
patch --dry-run -p1 < /path/to/php-7.2.34-security-backport.patch
patch -p1 < /path/to/php-7.2.34-security-backport.patch
```

Rebuild PHP using the configuration appropriate for your environment.

Do not apply this patch to a different PHP version without reviewing compatibility.

## Verification performed

The following checks were performed in an isolated Ubuntu laboratory:

- Patch dry-run succeeded against original PHP 7.2.34.
- All 17 modified files were compared against the corresponding Sury-patched sources.
- Patched PHP compiled successfully.
- Zend tests: 2,396 PASS, 52 SKIP, 6 XFAIL, 0 FAIL.
- Basic RFC1867 multipart tests: 15 PASS, 0 FAIL.
- Apache testing confirmed that normal multipart uploads still work.
- Comparison with unpatched PHP confirmed that session upload progress is disabled.

These tests do not establish complete mitigation of both CVEs.

## Known functional limitation

The patch disables PHP session upload-progress tracking.

In `ext/session/session.c`, the following condition is introduced:

```c
if (!PS(rfc1867_enabled) || 1) {
    return retval;
}
```

Applications relying on `session.upload_progress` may lose that functionality.

Normal multipart file uploads continued to work in our tests.

## Security warning

PHP 7.2 is end-of-life and no longer receives official security updates.

This patch does not address every known PHP 7.2 vulnerability and is not a substitute for upgrading to a supported PHP version.

Independent security review and application-specific testing are strongly recommended before any production deployment.

## Patch SHA-256

`df697d7b5ca2f542beca38f1f3eae06300605d0291ba91543356b07c8d262a8e`

## Disclaimer

This repository provides a community-maintained backport for research and compatibility purposes.

It is not an official PHP security release and is not affiliated with the PHP project or the original patch authors.

## Backport Integration and Verification

**Gaspar Gerardo Dueñas Avilés**  
**AIZUINC S.A.P.I. de C.V. — Mexico**

Responsible for integrating, compiling, testing, and documenting this PHP 7.2.34 security backport.

The original security fixes remain credited to their respective authors: Arnaud Le Blanc, Ilija Tovilo, and Remi Collet.

This project is an independent verification and documentation effort, not an official PHP security release.
