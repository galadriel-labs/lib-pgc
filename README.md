# Kunlun

## Specifications

- OS: MAC OS x64, Linux x64
- Language: C++
- Requires: OpenSSL, OpenMP

## Install Depedent Libaraies

### On MACOS

- download the latest OpenSSL from the website, to support curve25519,
  modify crypto/ec/curve25519.c line 211: remove "static", then compile it:

```
  $ ./Configure darwin64-x86_64-cc shared enable-ec_nistp_64_gcc_128 no-ssl2 no-ssl3 no-comp --openssldir=/usr/local/ssl/macos-x86_64
  $ make depend
  $ sudo make install
```

test if the function x25519_scalar_mulx is available

```
  $ cd /usr/local/lib
  $ nm libcrypto.a | grep x25519_scalar_mulx
```

- install OpenMP

```
  $ brew install libomp
```

### On Linux

- install OpenSSL 3.0

do the same modification as in MACOS, then compile it according to

```
  $ ./Configure no-shared enable-ec_nistp_64_gcc_128 no-ssl2 no-ssl3 no-comp --prefix=/usr/local/openssl
  $ make depend
  $ sudo make install
```

if reporting cannot find "opensslv.h" error, try to install libssl-dev

```
  $ sudo apt-get install libssl-dev
```

- install OpenMP

```
  $ sudo apt-get install libomp-dev
```

## Compile and Run

```
  $ mkdir build && cd build
  $ cmake ..
  $ make
  $ ./test_xxx
```

---

## License

This library is licensed under the [MIT License](LICENSE).

---
