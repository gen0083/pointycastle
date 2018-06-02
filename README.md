Pointy Castle
=============

[![pub package](https://img.shields.io/pub/v/pointycastle.svg)](https://pub.dartlang.org/packages/pointycastle) [![pub package](https://img.shields.io/badge/dynamic/json.svg?label=pub&url=https%3A%2F%2Fpub.dartlang.org%2Fpackages%2Fpointycastle.json&query=%24.versions%5B%3F(%40%3D%3D%221.0.0-rc1%22)%5D&colorB=fe7d37)](https://pub.dartlang.org/packages/pointycastle/versions/1.0.0-rc1)

A Dart library for encryption and decryption. As of today, most of the classes
are ports of Bouncy Castle from Java to Dart. The porting is almost always
direct except for some classes that had been added to ease the use of low level
data.

To make sure nothing fails, tests and benchmarks for every algorithm are
provided. The expected results are taken from the Bouncy Castle Java version
and also from standards, and matched against the results got from Pointy Castle.

# Algorithms

As of the last release, the following algorithms are implemented:

**Block ciphers:**
  * AES

**Asymmetric block ciphers:**
  * RSA

**Stream ciphers:**
  * Salsa20

**Block cipher modes of operation:**
  * CBC (Cipher Block Chaining mode)
  * CFB (Cipher Feedback mode)
  * ECB (Electronic Code Book mode)
  * GCTR (GOST 28147 OFB counter mode)
  * OFB (Output FeedBack mode)
  * CTR (Counter mode)
  * SIC

**Paddings:**
  * PKCS7

**Digests:**
  * MD2
  * MD4
  * MD5
  * RIPEMD-128
  * RIPEMD-160
  * RIPEMD-256
  * RIPEMD-320
  * SHA-1
  * SHA-224
  * SHA-256
  * SHA-3
  * SHA-384
  * SHA-512
  * SHA-512/t
  * Tiger
  * Whirlpool

**MACs:**
  * HMAC

**Signatures:**
  * (DET-)ECDSA
  * RSA

**Password based key derivators:**
  * PBKDF2
  * scrypt

**Asymmetric key generators:**
  * ECDSA
  * RSA

**Secure PRNGs:**
  * Based on block cipher in CTR mode
  * Based on block cipher in CTR mode with auto reseed (for forward security)
  * Based on Fortuna algorithm


# Usage

There are two ways to use the algorithms that PointyCastle provides: with or
without using the registry.

## Registry

The registry allows users to easily instantiate classes for the algorithms using
the algorithm shorthands like given in the list above.  It also makes it possible
to seamlessly chain different algorithms together.  For example:

```dart
import "package:pointycastle/pointycastle.dart";

void main() {
  Digest sha256 = new Digest("SHA-256");
  // or
  KeyDerivator derivator = new KeyDerivator("SHA-1/HMAC/PBKDF2");
}
```

## Without the registry

Using the registry means that all algorithms will be imported by default, which
can possibly increase the compiled size of your program.  To avoid this, it is
possible to import algorithms one by one.  In that case, you can decide to either
use the classes directly, or still use the registry.  But remember that the
registry only contains the classes that you import.  For example:

```dart
import "package:pointycastle/api.dart";

import "package:pointycastle/digests/sha256.dart";

import "package:pointycastle/digests/sha1.dart";
import "package:pointycastle/macs/hmac.dart";
import "package:pointycastle/key_derivators/pbkdf2.dart";

void main() {
  Digest sha256 = new SHA256Digest();
  // or
  KeyDerivator derivator = new PBKDF2KeyDerivator(
      new HMac(new SHA1Digest(), 64));

  // But the registry keeps working for all imported algorithms:

  Digest sha256 = new Digest("SHA-256");
  // or
  KeyDerivator derivator = new KeyDerivator("SHA-1/HMAC/PBKDF2");
}
```

## Without dart:mirrors or package:reflectable (Flutter)

Because the registry uses reflectable to register all imported algorithm classes
and reflectable imports `dart:mirrors`, using the registry depends on the ability
to import mirrors.  Since the [Flutter](https://flutter.io) frame work
[doesn't allow using dart:mirrors](https://github.com/flutter/flutter/issues/1150),
it's not possible to use the registry with Flutter.

The way to use Pointy Castle in Flutter is the way explained in the previous
section.  However, there is a utility library that exports all available
algorithms at once.

```dart
import "package:pointycastle/export.dart";

void main() {
  Digest sha256 = new SHA256Digest();
  // or
  KeyDerivator derivator = new PBKDF2KeyDerivator(
      new HMac(new SHA1Digest(), 64));
}
```


## Libraries

 * `package:pointycastle/pointycastle.dart`: exports the high-level API and the
    registry loaded with all available implementations
 * `package:pointycastle/api.dart`: exports the high-level API and the registry
    without any implementations
 * `package:pointycastle/export.dart`: exports the API and all implementation
    classes
