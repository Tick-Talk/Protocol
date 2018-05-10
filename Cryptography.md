# Use libsodium for all application needs
Important links relating to development:
* See https://download.libsodium.org/doc/ for libsodium docs
* See https://paragonie.com/book/pecl-libsodium for a book describing usage
* See https://dev.to/paragonie/libsodium-quick-reference for libsodium quick reference

Suggested libsodium bindings for development:
* Swift (iOS): https://github.com/jedisct1/swift-sodium
* Java (Android): https://github.com/joshjdevl/libsodium-jni
* Java (Server): https://github.com/abstractj/kalium
* JavaScript (Web Client): https://github.com/jedisct1/libsodium.js

# Password Hashing
* We hash passwords with sodium_crypto_pwhash
  * `sodium_crypto_pwhash_str` is used to create a password hash
  * `sodium_crypto_pwhash_str_verify` is used to verify a password hash
* TODO: Choose constants to use
  * `SODIUM_CRYPTO_PWHASH_OPSLIMIT_SENSITIVE`
  * `SODIUM_CRYPTO_PWHASH_MEMLIMIT_SENSITIVE`

# Transmitting Data (Public Key Encryption)
* We use authenticated Public Key Encryption by using `sodium_crypto_box`
* See https://paragonie.com/book/pecl-libsodium/read/05-publickey-crypto.md for usage
