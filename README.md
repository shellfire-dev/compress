# `compress`: functions module for [shellfire]

This module provides a simple framework for working with compressions such as `gzip` and `xz`. It abstracts away most details, providing a consistent interface to all forms of compression. It focuses entirely on compressing files with maximum compression. In doing so, wherever possible, it eliminates file meta data (eg timestamps). It preserves original, uncompressed files. Wherever possible, it uses the optimum tool and 'turns the dial upto 11' but can fallback as appropriate (eg `pigz` for `gzip`, falling back to `zopfli` and then `gzip` proper). Functions exist to determine file extensions and MIME types for a compressor. It does not support decompression.

An example user is the [swaddle] packaging tool.

## Overview

For example, to maximally compress a file with gzip:-

```bash
compress_gzip '/path/to/file'
```

This will output the file '/path/to/file.gz' and keep '/path/to/file'. No metadata will be kept and maximal compression will be used. The compressed file's timestamps will match the original's.

Functions are sanely named, so for example the following works:-

```bash
for compressor in gzip bzip2 xz lrzip
do
	compress_${compressor} '/path/to/file'
done
```

Currently, the following compressors are defined (in rough compressive power order):-

* `lzop`
* `zip` (not a typo; creates a single-file zip archive)
* `gzip`
* `bzip2`
* `rzip`
* `lzma`
* `xz`
* `lzip`
* `zpaq`
* `lrzip` (insane crushing potential)

Compressors choose amongst the available alternatives on any system, and try to install using the dependency framework in [core] the preferred compressor. Each compressor is in its own functions file, so it needn't pollute your code if not needed.

## Importing

To import this module, add a git submodule to your repository. From the root of your git repository in the terminal, type:-
```bash
mkdir -p lib/shellfire
cd lib/shellfire
git submodule add "https://github.com/shellfire-dev/compress.git"
cd -
git submodule init --update
```

You may need to change the url `https://github.com/shellfire-dev/compress.git"` above if using a fork.

You will also need to add paths - include the module [paths.d].


## Namespace `compress`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn compress
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn compress
	…
}
```

### Functions

***
#### `compress_extension`
|Parameter|Value|Optional|
|---------|-----|--------|
|`compressor`|Compressor as defined in Overview, or `none`|_No_|

Returns a file extension string. Result is printed to standard out _without_ an initial period, so use captures, eg

```bash
local fileExtensionExcludingInitialPeriod="$(compress_extension 'gzip')"
```

***
#### `compress_mimeType`
|Parameter|Value|Optional|
|---------|-----|--------|
|`compressor`|Compressor as defined in Overview, or `none`|_No_|

Returns a MIME type string. Result is printed to standard out _without_ an initial period, so use captures, eg

```bash
local mimeType="$(compress_mimeType 'gzip')"
```

***

## Namespace `compress_bzip2`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn compress bzip2
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn compress bzip2
	…
}
```

### Functions

***
#### `compress_bzip2`
|Parameter|Value|Optional|
|---------|-----|--------|
|`uncompressedFilePath`|Extant uncompressed file path|_No_|

Compresses `uncompressedFilePath` to `uncompressedFilePath` + `.$(compress_extension bzip2)`. Preserves `uncompressedFilePath`. Sets the compressed file's timestamp to match the uncompressed original.

```bash
compress_bzip2 '/path/to/file'
```


***
## Namespace `compress_gzip`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn compress gzip
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn compress gzip
	…
}
```

### Functions

***
#### `compress_gzip`
|Parameter|Value|Optional|
|---------|-----|--------|
|`uncompressedFilePath`|Extant uncompressed file path|_No_|

Compresses `uncompressedFilePath` to `uncompressedFilePath` + `.$(compress_extension gzip)`. Preserves `uncompressedFilePath`. Sets the compressed file's timestamp to match the uncompressed original.

```bash
compress_gzip '/path/to/file'
```

***
## Namespace `compress_lrzip`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn compress lrzip
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn compress lrzip
	…
}
```

### Functions

***
#### `compress_lrzip`
|Parameter|Value|Optional|
|---------|-----|--------|
|`uncompressedFilePath`|Extant uncompressed file path|_No_|

Compresses `uncompressedFilePath` to `uncompressedFilePath` + `.$(compress_extension lrzip)`. Preserves `uncompressedFilePath`. Sets the compressed file's timestamp to match the uncompressed original.

```bash
compress_lrzip '/path/to/file'
```

***
## Namespace `compress_lzip`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn compress lzip
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn compress lzip
	…
}
```

### Functions

***
#### `compress_lzip`
|Parameter|Value|Optional|
|---------|-----|--------|
|`uncompressedFilePath`|Extant uncompressed file path|_No_|

Compresses `uncompressedFilePath` to `uncompressedFilePath` + `.$(compress_extension lzip)`. Preserves `uncompressedFilePath`. Sets the compressed file's timestamp to match the uncompressed original.

```bash
compress_lzip '/path/to/file'
```


***

## Namespace `compress_lzma`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn compress lzma
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn compress lzma
	…
}
```

### Functions

***
#### `compress_lzma`
|Parameter|Value|Optional|
|---------|-----|--------|
|`uncompressedFilePath`|Extant uncompressed file path|_No_|

Compresses `uncompressedFilePath` to `uncompressedFilePath` + `.$(compress_extension lzma)`. Preserves `uncompressedFilePath`. Sets the compressed file's timestamp to match the uncompressed original.

```bash
compress_lzma '/path/to/file'
```

***

## Namespace `compress_lzop`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn compress lzop
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn compress lzop
	…
}
```

### Functions

***
#### `compress_lzop`
|Parameter|Value|Optional|
|---------|-----|--------|
|`uncompressedFilePath`|Extant uncompressed file path|_No_|

Compresses `uncompressedFilePath` to `uncompressedFilePath` + `.$(compress_extension lzop)`. Preserves `uncompressedFilePath`. Sets the compressed file's timestamp to match the uncompressed original.

```bash
compress_lzop '/path/to/file'
```


***
## Namespace `compress_rzip`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn compress rzip
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn compress rzip
	…
}
```

### Functions

***
#### `compress_rzip`
|Parameter|Value|Optional|
|---------|-----|--------|
|`uncompressedFilePath`|Extant uncompressed file path|_No_|

Compresses `uncompressedFilePath` to `uncompressedFilePath` + `.$(compress_extension rzip)`. Preserves `uncompressedFilePath`. Sets the compressed file's timestamp to match the uncompressed original.

```bash
compress_rzip '/path/to/file'
```

***


## Namespace `compress_xz`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn compress xz
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn compress xz
	…
}
```

### Functions

***
#### `compress_xz`
|Parameter|Value|Optional|
|---------|-----|--------|
|`uncompressedFilePath`|Extant uncompressed file path|_No_|

Compresses `uncompressedFilePath` to `uncompressedFilePath` + `.$(compress_extension xz)`. Preserves `uncompressedFilePath`. Sets the compressed file's timestamp to match the uncompressed original.

```bash
compress_xz '/path/to/file'
```


***
## Namespace `compress_zip`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn compress zip
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn compress zip
	…
}
```

### Functions

***
#### `compress_zip`
|Parameter|Value|Optional|
|---------|-----|--------|
|`uncompressedFilePath`|Extant uncompressed file path|_No_|

Compresses `uncompressedFilePath` to `uncompressedFilePath` + `.$(compress_extension zip)`. Preserves `uncompressedFilePath`. Sets the compressed file's timestamp to match the uncompressed original. Creates a ZIP archive with a single member.

```bash
compress_zip '/path/to/file'
```

***

## Namespace `compress_zpaq`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn compress zpaq
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn compress zpaq
	…
}
```

### Functions

***
#### `compress_zpaq`
|Parameter|Value|Optional|
|---------|-----|--------|
|`uncompressedFilePath`|Extant uncompressed file path|_No_|

Compresses `uncompressedFilePath` to `uncompressedFilePath` + `.$(compress_extension zpaq)`. Preserves `uncompressedFilePath`. Sets the compressed file's timestamp to match the uncompressed original. Creates a ZPAQ archive with a single member.

```bash
compress_zpaq '/path/to/file'
```








[swaddle]: https://github.com/raphaelcohn/swaddle "Swaddle homepage"
[shellfire]: https://github.com/shellfire-dev "shellfire homepage"
[core]: https://github.com/shellfire-dev/core "shellfire core module homepage"
[paths.d]: https://github.com/shellfire-dev/paths.d "paths.d shellfire module homepage"
