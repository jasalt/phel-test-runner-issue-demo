# Phel test runner failing to resolve Composer library function

- Phel v0.31.0-beta#6585706
- PHP 8.4.16 (Debian)

There seems to be issue with test runner resolving library functions like `php/amp\file\write` (from [Amp\File](https://github.com/amphp/file/blob/b7db7ca833da279b71b4fe52eeebbd55c8e9fc41/src/functions.php#L298)).

## Reproduction

Initialize project:

```
composer install
```

Running `core.phel` working as expected:

```
./vendor/bin/phel run ./src/phel/core.phel
Writing file "foo.txt" with Amp\File\write
Reading it with Amp\File\read
WORKS!
```

However, when running test for one function in that file, there's issue:

```
./vendor/bin/phel test
Call to undefined function app\core\amp\file\write() // file: /tmp/phel/cache/compiled/app_core.php
> Dont you see the file? Check your phel config got `KeepGeneratedTempFiles=true`
```

When the call to `(main)` is commented out, test runner works:

```
./vendor/bin/phel test
.

Passed: 1
Failed: 0
Error: 0
Total: 1
Time: 00:00.106, Memory: 20.00 MB
```

## Expected result

Test runner should not error with `Call to undefined function app\core\amp\file\write()`.
