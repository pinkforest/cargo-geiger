cargo-geiger ☢️ 
===============

[![Build Status](https://dev.azure.com/cargo-geiger/cargo-geiger/_apis/build/status/rust-secure-code.cargo-geiger?branchName=master)](https://dev.azure.com/cargo-geiger/cargo-geiger/_build/latest?definitionId=1&branchName=master)
[![unsafe forbidden](https://img.shields.io/badge/unsafe-forbidden-success.svg)](https://github.com/rust-secure-code/safety-dance/)
[![Code Coverage](https://img.shields.io/azure-devops/coverage/cargo-geiger/cargo-geiger/2/master)](https://img.shields.io/azure-devops/coverage/cargo-geiger/cargo-geiger/2/master)
[![crates.io](https://img.shields.io/crates/v/cargo-geiger.svg)](https://crates.io/crates/cargo-geiger)
[![Crates.io](https://img.shields.io/crates/d/cargo-geiger?label=cargo%20installs)](https://crates.io/crates/cargo-geiger)

A tool that lists statistics related to the usage of unsafe Rust code in a Rust
crate and all its dependencies.

This cargo plugin was originally based on the code from two other projects:
* <https://github.com/icefoxen/cargo-osha> and
* <https://github.com/sfackler/cargo-tree>

Installation
------------

Try to find and use a system-wide installed OpenSSL library:

```bash
cargo install cargo-geiger
```

Or, build and statically link OpenSSL as part of the cargo-geiger executable:

```bash
cargo install cargo-geiger --features vendored-openssl
```

Usage
-----

1. Navigate to the same directory as the `Cargo.toml` you want to analyze.
2. `cargo geiger`

Output example
--------------

![Example output](https://user-images.githubusercontent.com/3704611/53132247-845f7080-356f-11e9-9c76-a9498d4a744b.png)

Why the name?
-------------

<https://en.wikipedia.org/wiki/Geiger_counter>

Unsafe code, like ionizing radiation, is unavoidable in some situations and should be safely contained!

Intended Use
------------

This tool is not meant to advise directly whether the code ultimately is truly insecure or not.

The purpose of cargo-geiger is to provide statistical input to auditing e.g. with:

- [cargo-crev](https://crates.io/crates/cargo-crev)
- [safety-dance](https://github.com/rust-secure-code/safety-dance)

The use of unsafe is nuanced and necessary in some cases and any motivation to use it is outside the scope of cargo-geiger.

It is important that any reporting is handled with care and it is important to understand what unsafe in Rust means:

- [Reddit: The Stigma around Unsafe](https://www.reddit.com/r/rust/comments/y1u068/the_stigma_around_unsafe/)
- [YouTube: Rust NYC: Jon Gjengset - Demystifying unsafe code](https://youtu.be/QAz-maaH0KM)
- [Rust-lang: WG Unsafe Code Guidelines](https://github.com/rust-lang/unsafe-code-guidelines)

Known issues
------------

 - Unsafe code inside macros is not detected. Needs macro expansion(?).
 - Unsafe code generated by `build.rs` is probably not detected.
 - More on the GitHub issue tracker.

Roadmap
-------

 - ~~There should be no false negatives. All unsafe code should be
   identified.~~ This is probably too ambitious, but scanning for
   `#![forbid(unsafe_code)]` should be a reliable alternative (implemented since
   0.6.0). Please see the [changelog].
 - An optional whitelist file at the root crate level to specify crates that are
   trusted to use unsafe (should only have an effect when placed in the project's root).

Libraries
---------

Cargo Geiger exposes three libraries:

 - `cargo-geiger` - Unversioned and highly unstable library exposing the internals of the `cargo-geiger` binary. As such, any function contained within this library may be subject to change.
 - `cargo-geiger-serde` - A library containing the serializable report types
 - `geiger` - A library containing a few decoupled [cargo] components used by [cargo-geiger]

Changelog
---------

View the changelog [here](https://github.com/rust-secure-code/cargo-geiger/blob/master/CHANGELOG.md)

[cargo]: https://crates.io/crates/cargo
[cargo-geiger]: https://crates.io/crates/cargo-geiger
[changelog]: https://github.com/rust-secure-code/cargo-geiger/blob/master/CHANGELOG.md

## Cargo Geiger Safety Report
```

Metric output format: x/y
    x = unsafe code used by the build
    y = total unsafe code found in the crate

Symbols: 
    🔒  = No `unsafe` usage found, declares #![forbid(unsafe_code)]
    ❓  = No `unsafe` usage found, missing #![forbid(unsafe_code)]
    ☢️  = `unsafe` usage found

Functions  Expressions  Impls  Traits  Methods  Dependency

0/0        0/0          0/0    0/0     0/0      🔒  cargo-geiger 0.11.1
15/18      432/439      3/3    0/0     11/11    ☢️  ├── anyhow 1.0.40
0/26       0/623        0/6    0/0     0/5      ❓  │   └── backtrace 0.3.56
0/0        0/23         0/0    0/0     0/0      ❓  │       ├── addr2line 0.14.1
0/0        0/51         0/2    0/0     0/0      ❓  │       │   ├── gimli 0.23.0
0/0        37/42        1/1    0/0     0/0      ☢️  │       │   │   └── indexmap 1.6.2
2/2        1006/1098    16/19  0/0     35/39    ☢️  │       │   │       ├── hashbrown 0.9.1
0/0        4/4          0/0    0/0     0/0      ☢️  │       │   │       │   └── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │       │   │       │       └── serde_derive 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │       │   │       │           ├── proc-macro2 1.0.24
0/0        0/0          0/0    0/0     0/0      🔒  │       │   │       │           │   └── unicode-xid 0.2.1
0/0        0/0          0/0    0/0     0/0      ❓  │       │   │       │           ├── quote 1.0.9
0/0        0/0          0/0    0/0     0/0      ❓  │       │   │       │           │   └── proc-macro2 1.0.24
0/0        45/45        3/3    0/0     2/2      ☢️  │       │   │       │           └── syn 1.0.67
0/0        0/0          0/0    0/0     0/0      ❓  │       │   │       │               ├── proc-macro2 1.0.24
0/0        0/0          0/0    0/0     0/0      ❓  │       │   │       │               ├── quote 1.0.9
0/0        0/0          0/0    0/0     0/0      🔒  │       │   │       │               └── unicode-xid 0.2.1
0/0        4/4          0/0    0/0     0/0      ☢️  │       │   │       └── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │       │   ├── rustc-demangle 0.1.18
1/1        392/392      7/7    1/1     13/13    ☢️  │       │   └── smallvec 1.6.1
0/0        4/4          0/0    0/0     0/0      ☢️  │       │       └── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │       ├── cfg-if 1.0.0
0/19       10/311       0/0    0/0     5/27     ☢️  │       ├── libc 0.2.92
0/0        0/0          0/0    0/0     0/0      🔒  │       ├── miniz_oxide 0.4.4
0/0        0/0          0/0    0/0     0/0      🔒  │       │   └── adler 1.0.2
0/0        0/21         0/0    0/1     0/0      ❓  │       ├── object 0.23.0
5/6        108/156      0/0    0/0     0/0      ☢️  │       │   ├── crc32fast 1.2.1
0/0        0/0          0/0    0/0     0/0      ❓  │       │   │   └── cfg-if 1.0.0
4/4        129/129      2/2    0/0     2/2      ☢️  │       │   ├── flate2 1.0.20
0/0        0/0          0/0    0/0     0/0      ❓  │       │   │   ├── cfg-if 1.0.0
5/6        108/156      0/0    0/0     0/0      ☢️  │       │   │   ├── crc32fast 1.2.1
0/19       10/311       0/0    0/0     5/27     ☢️  │       │   │   ├── libc 0.2.92
0/0        0/0          0/0    0/0     0/0      ❓  │       │   │   ├── libz-sys 1.1.2
0/19       10/311       0/0    0/0     5/27     ☢️  │       │   │   │   └── libc 0.2.92
0/0        0/0          0/0    0/0     0/0      🔒  │       │   │   └── miniz_oxide 0.4.4
0/0        37/42        1/1    0/0     0/0      ☢️  │       │   └── indexmap 1.6.2
0/0        0/0          0/0    0/0     0/0      ❓  │       ├── rustc-demangle 0.1.18
0/0        4/4          0/0    0/0     0/0      ☢️  │       └── serde 1.0.125
4/4        341/347      0/0    0/0     3/3      ☢️  ├── cargo 0.52.0
15/18      432/439      3/3    0/0     11/11    ☢️  │   ├── anyhow 1.0.40
2/2        45/45        0/0    0/0     0/0      ☢️  │   ├── atty 0.2.14
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   └── libc 0.2.92
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── bytesize 1.0.1
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   └── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── cargo-platform 0.1.1
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   └── serde 1.0.125
0/0        1/1          0/0    0/0     0/0      ☢️  │   ├── clap 2.33.3
0/0        23/23        0/0    0/0     0/0      ☢️  │   │   ├── ansi_term 0.11.0
2/2        45/45        0/0    0/0     0/0      ☢️  │   │   ├── atty 0.2.14
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── bitflags 1.2.1
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── strsim 0.8.0
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── textwrap 0.11.0
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │   └── unicode-width 0.1.8
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── unicode-width 0.1.8
0/0        0/0          0/0    0/0     0/0      ❓  │   │   └── vec_map 0.8.2
0/0        4/4          0/0    0/0     0/0      ☢️  │   │       └── serde 1.0.125
0/0        606/606      12/12  4/4     12/12    ☢️  │   ├── core-foundation 0.9.1
0/0        3/3          0/0    0/0     2/2      ☢️  │   │   ├── core-foundation-sys 0.8.2
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   └── libc 0.2.92
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── crates-io 0.33.0
15/18      432/439      3/3    0/0     11/11    ☢️  │   │   ├── anyhow 1.0.40
4/4        875/876      5/5    0/0     2/2      ☢️  │   │   ├── curl 0.4.35
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │   ├── curl-sys 0.4.41+curl-7.75.0
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   │   │   ├── libc 0.2.92
0/0        0/1          0/0    0/0     0/0      ❓  │   │   │   │   ├── libnghttp2-sys 0.1.6+1.43.0
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   │   │   │   └── libc 0.2.92
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │   │   └── libz-sys 1.1.2
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   │   ├── libc 0.2.92
0/0        644/1122     0/0    0/0     5/9      ☢️  │   │   │   └── socket2 0.3.19
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │       ├── cfg-if 1.0.0
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   │       └── libc 0.2.92
0/0        3/3          0/0    0/0     0/0      ☢️  │   │   ├── percent-encoding 2.1.0
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   ├── serde 1.0.125
0/0        6/6          0/0    0/0     0/0      ☢️  │   │   ├── serde_json 1.0.64
0/0        37/42        1/1    0/0     0/0      ☢️  │   │   │   ├── indexmap 1.6.2
0/0        1/1          0/0    0/0     0/0      ☢️  │   │   │   ├── itoa 0.4.7
8/12       674/921      0/0    0/0     2/2      ☢️  │   │   │   ├── ryu 1.0.5
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   │   └── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │   │   └── url 2.2.1
0/0        2/2          0/0    0/0     0/0      ☢️  │   │       ├── form_urlencoded 1.0.1
0/0        0/0          0/0    0/0     0/0      ❓  │   │       │   ├── matches 0.1.8
0/0        3/3          0/0    0/0     0/0      ☢️  │   │       │   └── percent-encoding 2.1.0
0/0        0/0          0/0    0/0     0/0      ❓  │   │       ├── idna 0.2.2
0/0        0/0          0/0    0/0     0/0      ❓  │   │       │   ├── matches 0.1.8
0/0        0/0          0/0    0/0     0/0      🔒  │   │       │   ├── unicode-bidi 0.3.4
0/0        0/0          0/0    0/0     0/0      ❓  │   │       │   │   ├── matches 0.1.8
0/0        4/4          0/0    0/0     0/0      ☢️  │   │       │   │   └── serde 1.0.125
0/0        20/20        0/0    0/0     0/0      ☢️  │   │       │   └── unicode-normalization 0.1.17
0/0        0/0          0/0    0/0     0/0      🔒  │   │       │       └── tinyvec 1.1.1
0/0        4/4          0/0    0/0     0/0      ☢️  │   │       │           ├── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │   │       │           └── tinyvec_macros 0.1.0
0/0        0/0          0/0    0/0     0/0      ❓  │   │       ├── matches 0.1.8
0/0        3/3          0/0    0/0     0/0      ☢️  │   │       ├── percent-encoding 2.1.0
0/0        4/4          0/0    0/0     0/0      ☢️  │   │       └── serde 1.0.125
4/4        79/79        14/14  0/0     2/2      ☢️  │   ├── crossbeam-utils 0.8.3
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── cfg-if 1.0.0
0/0        7/7          1/1    0/0     0/0      ☢️  │   │   └── lazy_static 1.4.0
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── crypto-hash 0.3.4
0/0        23/23        0/0    0/0     0/0      ☢️  │   │   ├── commoncrypto 0.2.0
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │   └── commoncrypto-sys 0.2.0
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   │       └── libc 0.2.92
0/0        0/0          0/0    0/0     0/0      ❓  │   │   └── hex 0.3.2
4/4        875/876      5/5    0/0     2/2      ☢️  │   ├── curl 0.4.35
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── curl-sys 0.4.41+curl-7.75.0
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── env_logger 0.8.3
2/2        45/45        0/0    0/0     0/0      ☢️  │   │   ├── atty 0.2.14
0/0        0/0          0/0    0/0     0/0      🔒  │   │   ├── humantime 2.1.0
1/1        16/16        1/1    0/0     0/0      ☢️  │   │   ├── log 0.4.14
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │   ├── cfg-if 1.0.0
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   │   └── serde 1.0.125
0/0        34/34        1/2    0/0     2/2      ☢️  │   │   ├── regex 1.4.5
19/19      678/678      0/0    0/0     22/22    ☢️  │   │   │   ├── aho-corasick 0.7.15
26/27      1823/1896    0/0    0/0     0/0      ☢️  │   │   │   │   └── memchr 2.3.4
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   │   │       └── libc 0.2.92
26/27      1823/1896    0/0    0/0     0/0      ☢️  │   │   │   ├── memchr 2.3.4
0/0        0/0          0/0    0/0     0/0      🔒  │   │   │   └── regex-syntax 0.6.23
0/0        0/0          0/0    0/0     0/0      ❓  │   │   └── termcolor 1.1.2
0/0        35/78        0/0    0/0     0/0      ☢️  │   ├── filetime 0.2.14
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── cfg-if 1.0.0
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   └── libc 0.2.92
4/4        129/129      2/2    0/0     2/2      ☢️  │   ├── flate2 1.0.20
9/9        3745/3765    3/3    0/0     81/81    ☢️  │   ├── git2 0.13.17
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── bitflags 1.2.1
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   ├── libc 0.2.92
0/0        18/18        0/0    0/0     0/0      ☢️  │   │   ├── libgit2-sys 0.12.18+1.1.0
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   │   ├── libc 0.2.92
2/2        6/6          0/0    0/0     0/0      ☢️  │   │   │   ├── libssh2-sys 0.2.21
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   │   │   ├── libc 0.2.92
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │   │   ├── libz-sys 1.1.2
42/42      149/149      0/0    0/0     0/0      ☢️  │   │   │   │   └── openssl-sys 0.9.61
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   │   │       └── libc 0.2.92
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │   ├── libz-sys 1.1.2
42/42      149/149      0/0    0/0     0/0      ☢️  │   │   │   └── openssl-sys 0.9.61
1/1        16/16        1/1    0/0     0/0      ☢️  │   │   ├── log 0.4.14
0/0        0/0          0/0    0/0     0/0      ❓  │   │   └── url 2.2.1
1/1        17/19        0/0    0/0     0/0      ☢️  │   ├── git2-curl 0.14.1
4/4        875/876      5/5    0/0     2/2      ☢️  │   │   ├── curl 0.4.35
9/9        3745/3765    3/3    0/0     81/81    ☢️  │   │   ├── git2 0.13.17
1/1        16/16        1/1    0/0     0/0      ☢️  │   │   ├── log 0.4.14
0/0        0/0          0/0    0/0     0/0      ❓  │   │   └── url 2.2.1
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── glob 0.3.0
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── hex 0.4.3
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   └── serde 1.0.125
0/0        0/14         0/0    0/0     0/0      ❓  │   ├── home 0.5.3
0/0        0/0          0/0    0/0     0/0      🔒  │   ├── humantime 2.1.0
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── ignore 0.4.17
4/4        79/79        14/14  0/0     2/2      ☢️  │   │   ├── crossbeam-utils 0.8.3
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── globset 0.4.6
19/19      678/678      0/0    0/0     22/22    ☢️  │   │   │   ├── aho-corasick 0.7.15
8/8        377/377      0/0    0/0     0/0      ☢️  │   │   │   ├── bstr 0.2.15
0/0        7/7          1/1    0/0     0/0      ☢️  │   │   │   │   ├── lazy_static 1.4.0
26/27      1823/1896    0/0    0/0     0/0      ☢️  │   │   │   │   ├── memchr 2.3.4
0/0        225/225      5/5    1/1     14/14    ☢️  │   │   │   │   ├── regex-automata 0.1.9
0/1        176/193      0/0    0/0     0/0      ☢️  │   │   │   │   │   ├── byteorder 1.4.3
0/0        0/0          0/0    0/0     0/0      🔒  │   │   │   │   │   └── regex-syntax 0.6.23
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   │   │   └── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │   ├── fnv 1.0.7
1/1        16/16        1/1    0/0     0/0      ☢️  │   │   │   ├── log 0.4.14
0/0        34/34        1/2    0/0     2/2      ☢️  │   │   │   ├── regex 1.4.5
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   │   └── serde 1.0.125
0/0        7/7          1/1    0/0     0/0      ☢️  │   │   ├── lazy_static 1.4.0
1/1        16/16        1/1    0/0     0/0      ☢️  │   │   ├── log 0.4.14
26/27      1823/1896    0/0    0/0     0/0      ☢️  │   │   ├── memchr 2.3.4
0/0        34/34        1/2    0/0     2/2      ☢️  │   │   ├── regex 1.4.5
0/0        3/3          0/0    0/0     0/0      ☢️  │   │   ├── same-file 1.0.6
0/0        109/109      1/1    0/0     4/4      ☢️  │   │   ├── thread_local 1.1.3
1/1        75/94        4/6    0/0     2/3      ☢️  │   │   │   └── once_cell 1.7.2
0/0        0/0          0/0    0/0     0/0      ❓  │   │   └── walkdir 2.3.2
0/0        3/3          0/0    0/0     0/0      ☢️  │   │       └── same-file 1.0.6
1/1        122/122      2/2    0/0     4/4      ☢️  │   ├── im-rc 15.0.0
0/0        100/100      0/0    0/0     9/9      ☢️  │   │   ├── bitmaps 2.1.0
0/0        0/0          0/0    0/0     0/0      🔒  │   │   │   └── typenum 1.13.0
0/0        22/22        0/0    0/0     0/0      ☢️  │   │   ├── rand_core 0.5.1
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   │   └── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── rand_xoshiro 0.4.0
0/0        22/22        0/0    0/0     0/0      ☢️  │   │   │   ├── rand_core 0.5.1
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   │   └── serde 1.0.125
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   ├── serde 1.0.125
0/1        311/631      0/0    0/0     20/39    ☢️  │   │   ├── sized-chunks 0.6.4
0/0        100/100      0/0    0/0     9/9      ☢️  │   │   │   ├── bitmaps 2.1.0
0/0        0/0          0/0    0/0     0/0      🔒  │   │   │   └── typenum 1.13.0
0/0        0/0          0/0    0/0     0/0      🔒  │   │   └── typenum 1.13.0
0/0        188/282      0/2    0/0     4/6      ☢️  │   ├── jobserver 0.1.21
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   └── libc 0.2.92
0/0        7/7          1/1    0/0     0/0      ☢️  │   ├── lazy_static 1.4.0
0/0        43/43        2/2    0/0     0/0      ☢️  │   ├── lazycell 1.3.0
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   └── serde 1.0.125
0/19       10/311       0/0    0/0     5/27     ☢️  │   ├── libc 0.2.92
0/0        18/18        0/0    0/0     0/0      ☢️  │   ├── libgit2-sys 0.12.18+1.1.0
1/1        16/16        1/1    0/0     0/0      ☢️  │   ├── log 0.4.14
26/27      1823/1896    0/0    0/0     0/0      ☢️  │   ├── memchr 2.3.4
0/0        65/72        0/0    0/0     0/0      ☢️  │   ├── num_cpus 1.13.0
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   └── libc 0.2.92
0/0        6/6          0/0    0/0     0/0      ☢️  │   ├── opener 0.4.1
30/30      5630/5630    33/33  3/3     16/16    ☢️  │   ├── openssl 0.10.33
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── bitflags 1.2.1
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── cfg-if 1.0.0
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── foreign-types 0.3.2
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │   └── foreign-types-shared 0.1.1
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   ├── libc 0.2.92
1/1        75/94        4/6    0/0     2/3      ☢️  │   │   ├── once_cell 1.7.2
42/42      149/149      0/0    0/0     0/0      ☢️  │   │   └── openssl-sys 0.9.61
0/0        3/3          0/0    0/0     0/0      ☢️  │   ├── percent-encoding 2.1.0
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── rustc-workspace-hack 1.0.0
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── rustfix 0.5.1
15/18      432/439      3/3    0/0     11/11    ☢️  │   │   ├── anyhow 1.0.40
1/1        16/16        1/1    0/0     0/0      ☢️  │   │   ├── log 0.4.14
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   ├── serde 1.0.125
0/0        6/6          0/0    0/0     0/0      ☢️  │   │   └── serde_json 1.0.64
0/0        3/3          0/0    0/0     0/0      ☢️  │   ├── same-file 1.0.6
0/0        0/4          0/0    0/0     0/0      ❓  │   ├── semver 0.10.0
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── semver-parser 0.7.0
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   └── serde 1.0.125
0/0        4/4          0/0    0/0     0/0      ☢️  │   ├── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── serde_ignored 0.1.2
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   └── serde 1.0.125
0/0        6/6          0/0    0/0     0/0      ☢️  │   ├── serde_json 1.0.64
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── shell-escape 0.1.5
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── strip-ansi-escapes 0.1.0
0/0        4/5          0/0    0/0     0/0      ☢️  │   │   └── vte 0.3.3
1/1        5/5          0/0    0/0     0/0      ☢️  │   │       └── utf8parse 0.1.1
2/2        52/52        0/0    0/0     0/0      ☢️  │   ├── tar 0.4.33
0/0        35/78        0/0    0/0     0/0      ☢️  │   │   ├── filetime 0.2.14
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   └── libc 0.2.92
0/0        36/82        0/0    0/0     0/0      ☢️  │   ├── tempfile 3.2.0
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── cfg-if 1.0.0
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   ├── libc 0.2.92
0/0        20/20        0/0    0/0     0/0      ☢️  │   │   ├── rand 0.8.3
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   │   ├── libc 0.2.92
1/1        16/16        1/1    0/0     0/0      ☢️  │   │   │   ├── log 0.4.14
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │   ├── rand_chacha 0.3.0
2/2        565/641      0/0    0/0     14/22    ☢️  │   │   │   │   ├── ppv-lite86 0.2.10
0/0        15/15        0/0    0/0     0/0      ☢️  │   │   │   │   └── rand_core 0.6.2
1/4        47/144       1/1    0/0     3/3      ☢️  │   │   │   │       ├── getrandom 0.2.2
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │   │       │   ├── cfg-if 1.0.0
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   │   │       │   └── libc 0.2.92
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   │   │       └── serde 1.0.125
0/0        15/15        0/0    0/0     0/0      ☢️  │   │   │   ├── rand_core 0.6.2
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   │   └── serde 1.0.125
0/0        0/79         0/0    0/0     0/0      ❓  │   │   └── remove_dir_all 0.5.3
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── termcolor 1.1.2
0/0        0/0          0/0    0/0     0/0      🔒  │   ├── toml 0.5.8
0/0        37/42        1/1    0/0     0/0      ☢️  │   │   ├── indexmap 1.6.2
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   └── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── unicode-width 0.1.8
0/0        0/0          0/0    0/0     0/0      🔒  │   ├── unicode-xid 0.2.1
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── url 2.2.1
0/0        0/0          0/0    0/0     0/0      ❓  │   └── walkdir 2.3.2
0/0        0/0          0/0    0/0     0/0      🔒  ├── cargo-geiger-serde 0.2.0
0/0        0/4          0/0    0/0     0/0      ❓  │   ├── semver 0.11.0
0/0        0/0          0/0    0/0     0/0      ❓  │   │   ├── semver-parser 0.10.2
2/2        57/57        0/0    0/0     2/2      ☢️  │   │   │   └── pest 2.1.3
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   │       ├── serde 1.0.125
0/0        6/6          0/0    0/0     0/0      ☢️  │   │   │       ├── serde_json 1.0.64
0/0        0/0          0/0    0/0     0/0      ❓  │   │   │       └── ucd-trie 0.1.3
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   └── serde 1.0.125
0/0        4/4          0/0    0/0     0/0      ☢️  │   ├── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │   └── url 2.2.1
0/0        0/0          0/0    0/0     0/0      ❓  ├── cargo-platform 0.1.1
0/0        0/0          0/0    0/0     0/0      ❓  ├── cargo_metadata 0.13.1
1/1        50/50        0/0    0/0     2/2      ☢️  │   ├── camino 1.0.4
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   └── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── cargo-platform 0.1.1
0/0        0/4          0/0    0/0     0/0      ❓  │   ├── semver 0.11.0
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── semver-parser 0.10.2
0/0        4/4          0/0    0/0     0/0      ☢️  │   ├── serde 1.0.125
0/0        6/6          0/0    0/0     0/0      ☢️  │   └── serde_json 1.0.64
0/0        13/13        0/0    0/0     0/0      ☢️  ├── colored 2.0.0
2/2        45/45        0/0    0/0     0/0      ☢️  │   ├── atty 0.2.14
0/0        7/7          1/1    0/0     0/0      ☢️  │   └── lazy_static 1.4.0
0/1        55/235       0/0    0/0     0/0      ☢️  ├── console 0.14.1
0/0        7/7          1/1    0/0     0/0      ☢️  │   ├── lazy_static 1.4.0
0/19       10/311       0/0    0/0     5/27     ☢️  │   ├── libc 0.2.92
0/0        34/34        1/2    0/0     2/2      ☢️  │   ├── regex 1.4.5
0/0        5/12         0/0    0/0     0/0      ☢️  │   ├── terminal_size 0.1.16
0/19       10/311       0/0    0/0     5/27     ☢️  │   │   └── libc 0.2.92
0/0        0/0          0/0    0/0     0/0      ❓  │   └── unicode-width 0.1.8
0/0        0/0          0/0    0/0     0/0      🔒  ├── geiger 0.4.6
0/0        0/0          0/0    0/0     0/0      🔒  │   ├── cargo-geiger-serde 0.2.0
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── proc-macro2 1.0.24
0/0        45/45        3/3    0/0     2/2      ☢️  │   └── syn 1.0.67
0/0        0/0          0/0    0/0     0/0      ❓  ├── krates 0.7.0
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── cargo_metadata 0.13.1
0/0        0/0          0/0    0/0     0/0      ❓  │   ├── cfg-expr 0.7.4
1/1        392/392      7/7    1/1     13/13    ☢️  │   │   └── smallvec 1.6.1
2/2        75/75        4/4    1/1     1/1      ☢️  │   ├── petgraph 0.5.1
0/0        62/62        0/0    0/0     0/0      ☢️  │   │   ├── fixedbitset 0.2.0
0/0        37/42        1/1    0/0     0/0      ☢️  │   │   ├── indexmap 1.6.2
0/0        4/4          0/0    0/0     0/0      ☢️  │   │   ├── serde 1.0.125
0/0        0/0          0/0    0/0     0/0      ❓  │   │   └── serde_derive 1.0.125
0/0        0/4          0/0    0/0     0/0      ❓  │   └── semver 0.11.0
2/2        75/75        4/4    1/1     1/1      ☢️  ├── petgraph 0.5.1
0/0        0/0          0/0    0/0     0/0      🔒  ├── pico-args 0.4.0
0/0        34/34        1/2    0/0     2/2      ☢️  ├── regex 1.4.5
0/0        4/4          0/0    0/0     0/0      ☢️  ├── serde 1.0.125
0/0        6/6          0/0    0/0     0/0      ☢️  ├── serde_json 1.0.64
0/0        0/0          0/0    0/0     0/0      ❓  ├── strum 0.20.0
0/0        0/0          0/0    0/0     0/0      ❓  │   └── strum_macros 0.20.1
0/0        0/0          0/0    0/0     0/0      ❓  │       ├── heck 0.3.2
0/0        0/0          0/0    0/0     0/0      ❓  │       │   └── unicode-segmentation 1.7.1
0/0        0/0          0/0    0/0     0/0      ❓  │       ├── proc-macro2 1.0.24
0/0        0/0          0/0    0/0     0/0      ❓  │       ├── quote 1.0.9
0/0        45/45        3/3    0/0     2/2      ☢️  │       └── syn 1.0.67
0/0        0/0          0/0    0/0     0/0      ❓  ├── strum_macros 0.20.1
0/0        0/0          0/0    0/0     0/0      ❓  ├── url 2.2.1
0/0        0/0          0/0    0/0     0/0      ❓  └── walkdir 2.3.2

200/260    20550/23557  121/137 10/11   296/361

```
