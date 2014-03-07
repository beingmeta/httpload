AC_INIT(port.h)
AC_PREFIX_PROGRAM(autoconf)
AC_CONFIG_HEADER(config.h)
AC_CONFIG_AUX_DIR(etc)

VERSION=0.2
AC_SUBST(VERSION)

AC_CHECK_HEADERS(openssl/ssl.h openssl/rand.h openssl/err.h openssl/crypto.h)
AC_CHECK_LIB(ssl,SSL_write)
AC_CHECK_LIB(crypto,EVP_CIPHER_CTX_init)

AC_OUTPUT(Makefile version.h)
