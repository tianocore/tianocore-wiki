# Third-Party Security Advisories

## [CVE-2025-11187 – OpenSSL](https://openssl-library.org/news/vulnerabilities/#CVE-2025-11187)

### Published: 1/27/2026

### Recommendation:
Not a problem for EDK2. EDK2 CryptoPkg does not support PKCS#12.

## [CVE-2025-15467 – OpenSSL](https://openssl-library.org/news/vulnerabilities/#CVE-2025-15467)

### Published: 1/27/2026

### Recommendation:

Not a problem for EDK2. EDK2 CryptoPkg does not support CMS.

## [CVE-2025-15468 – OpenSSL](https://openssl-library.org/news/vulnerabilities/#CVE-2025-15468)

### Published: 1/27/2026

### Recommendation:

Not a problem for EDK2. EDK2 CryptoPkg does not use SSL_CIPHER_find().

## [CVE-2025-15469 – OpenSSL](https://openssl-library.org/news/vulnerabilities/#CVE-2025-15469)

### Published: 1/27/2026

### Recommendation:

Not a problem for EDK2. EDK2 CryptoPkg does not use OpenSSL applications, including the "openssl dgst" command-line tool.

## [CVE-2025-66199 – OpenSSL](https://openssl-library.org/news/vulnerabilities/#CVE-2025-66199)

### Published: 1/27/2026

### Recommendation:

Not a problem for EDK2. EDK2 CryptoPkg does not support CompressedCertificate. All compression algorithms (brotli, zlib, zstd) are disabled by default.

## [CVE-2025-68160 – OpenSSL](https://openssl-library.org/news/vulnerabilities/#CVE-2025-68160)

### Published: 1/27/2026

### Recommendation:

Not a problem for EDK2. EDK2 CryptoPkg does not have a use case for BIO_f_linebuffer.

## [CVE-2025-69418 – OpenSSL](https://openssl-library.org/news/vulnerabilities/#CVE-2025-69418)

### Published: 1/27/2026

### Recommendation:

Not a problem for EDK2. EDK2 CryptoPkg has OCB disabled by default.

## [CVE-2025-69419 – OpenSSL](https://openssl-library.org/news/vulnerabilities/#CVE-2025-69419)

### Published: 1/27/2026

## Recommendation:

Not a problem for EDK2. EDK2 CryptoPkg does not support PKCS#12.

## [CVE-2025-69420 – OpenSSL](https://openssl-library.org/news/vulnerabilities/#CVE-2025-69420)

### Published: 1/27/2026

### Recommendation:

Not a problem for EDK2. EDK2 CryptoPkg has TS disabled by default.

## [CVE-2025-69421 – OpenSSL](https://openssl-library.org/news/vulnerabilities/#CVE-2025-69421)

### Published: 1/27/2026

### Recommendation:

Not a problem for EDK2. EDK2 CryptoPkg does not support PKCS#12.

## [CVE-2025-9232- OpenSSL](https://www.cve.org/CVERecord?id=CVE-2025-9232)

### Published: 9/30/2025

### Recommendation

Not a problem for EDK2. The OpenSSL HTTP implementation is not used by EDK2. EDK2 has its own HTTP implementation in
NetworkPkg.

## [CVE-2025-9231- OpenSSL](https://www.cve.org/CVERecord?id=CVE-2025-9231)

### Published: 9/30/2025

### Recommendation

Not a problem for EDK2. SM2 is not used by EDK2 CryptoPkg.

## [CVE-2025-9230- OpenSSL](https://www.cve.org/CVERecord?id=CVE-2025-9230)

## Published: 9/30/2025

## Recommendation

Not a problem for EDK2. CMS is not used by EDK2 CryptoPkg.

## [CVE-2024-12797- OpenSSL](https://www.cve.org/CVERecord?id=CVE-2024-12797)

## Published: 2/11/2025

## Recommendation

Not a problem for EDK2. The OpenSSL API to enable Raw Public Keys (RPKs) is not available to users of EDK2 CryptoPkg.

## [CVE-2024-9143 - OpenSSL](https://www.cve.org/CVERecord?id=CVE-2024-9143)

## Published: 10/16/2024

## Recommendation

Not a problem for EDK2. GF(2^M) Curves (EC2M) are not enabled in EDK2 CryptoPkg.

## [CVE-2024-6119 - OpenSSL](https://www.cve.org/CVERecord?id=CVE-2024-6119)

## Published: 9/3/2024

## Recommendation

Impacts EDK2 TlsLib. The issue is only applicable to OpenSSL 3.* branches. Recommend moving to OpenSSL 3.0.15.

The issue does not impact OpenSSL 1.1.1 branch.

## [CVE-2024-5535 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2024-5535)

## Published: 6/27/2024

## Recommendation

Not a problem for EDK2. EDK2 does not call the SSL_select_next_proto function.

## [CVE-2024-4741 - OpenSSL](https://www.openssl.org/news/secadv/20240528.txt)

## Published: 5/28/2024

## Recommendation

Not a problem for EDK2. EDK2 does not directly call the SSL_free_buffers function.

## [CVE-2024-4603 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2024-4603)

## Published: 5/16/2024

## Recommendation

Not a problem for EDK2. EDK2 does not support DSA.

## [CVE-2024-2511 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2024-2511)

## Published: 4/08/2024

## Recommendation

Not a problem for EDK2. This CVE only affects TLS servers supporting TLSv1.3. EDK2 does not support TLS server
configurations. EDK2 does not support TLSv1.3.

## [CVE-2024-0727 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2024-0727)

## Published: 1/25/2024

## Recommendation

Not a problem for EDK2. EDK2 CryptoPkg does not use the PKCS12 implementation or the SMIME_write_PKCS7() function.

## [CVE-2023-6237 - OpenSSL](https://www.openssl.org/news/secadv/20240115.txt)

## Published: 1/15/2024

## Recommendation

No impact. The EDK2 CryptoPkg does not use the OpenSSL function EVP_PKEY_public_check().

## [CVE-2023-6129 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-6129)

## Published: 1/09/2024

## Recommendation

No impact. The OpenSSL POLY1305 implementation is disabled by default in the EDK2 CryptoPkg configuration.

## [CVE-2023-5678 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-5678)

## Published: 11/06/2023

## Recommendation

No impact. The affected DH params are not used by EDK2. EDK2 does not call any DH_check* related functions.

## [CVE-2023-5363 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-5363)

## Published: 10/25/2023

## Recommendation

No impact. The affected functions that change the key and IV lengths are not called by EDK2 CryptoPkg.

## [CVE-2023-4807 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-4807)

## Published: 09/08/2023

## Recommendation

No impact.
The OpenSSL POLY1305 implementation is disabled by default in the CryptoPkg configuration.

## [CVE-2023-3817 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-3817)

## Published: 07/31/2023

## Recommendation

No impact.
DH code is used in CryptDh.c and TlsLib. In CryptoDh.c, EDK2 does not use DH_check() related functions and existing
functions will not call into DH_check(). The TlsLib implementation is not affected by this issue.

## [CVE-2023-3446 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-3446)

## Published: 07/19/2023

## Recommendation

No impact.
The DH code of EDK2 does not call the DH_check() and related functions, or have the risk of using “p” parameters that
are too large.

## [CVE-2023-2975 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-2975)

## Published: 07/14/2023

## Recommendation

No impact.
The OpenSSL AES-SIV cipher implementation is disabled by default in the CryptoPkg configuration.

## [CVE-2023-2650 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-2650)

## Published: 05/30/2023

## Recommendation

- OpenSSL 1.1.1u, updated in the edk2-stable202308 stable tag

## [CVE-2023-0466 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-0466)

## Published: 03/28/2023

## Recommendation

No Impact

EDK2 does not use the OpenSSL function X509_VERIFY_PARAM_add0_policy().

## [CVE-2023-0465 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-0465)

## Published: 03/28/2023

## Recommendation

No Impact

According to the CVE description, ‘policy’ processing is disabled by default.

For PKCS7, the X509 default parameter is used (NULL is passed in
CryptoPkg\Library\BaseCryptLib\Pk\CryptPkcs7VerifyCommon.c)

In X509_STORE_CTX_init(), ‘default’ parameter is used (CryptoPkg\Library\OpensslLib\openssl\crypto\x509\x509_vfy.c)

For X509 verify, no policy parameter is set (CryptoPkg\Library\BaseCryptLib\Pk\CryptX509.c)

## [CVE-2023-0464 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-0464)

## Published: 03/22/2023

## Recommendation

No Impact

According to the CVE description, ‘policy’ processing is disabled by default.

For PKCS7, the X509 default parameter is used (NULL is passed in
CryptoPkg\Library\BaseCryptLib\Pk\CryptPkcs7VerifyCommon.c)

In X509_STORE_CTX_init(), ‘default’ parameter is used (CryptoPkg\Library\OpensslLib\openssl\crypto\x509\x509_vfy.c)

For X509 verify, no policy parameter is set (CryptoPkg\Library\BaseCryptLib\Pk\CryptX509.c)

## [CVE-2023-0286 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-0286)

## Published: 02/08/2023

## Recommendation

No impact to EDK2 updating will resolve CVE

- OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag

## [CVE-2023-0215 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-0215)

## Published: 02/08/2023

## Recommendation

- OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag

## [CVE-2022-4450 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-4450)

## Published: 02/08/2023

## Recommendation

- OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag

## [CVE-2022-4304 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-4304)

## Published: 02/08/2023

## Recommendation

- OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag

## [CVE-2022-2097 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-2097)

## Published: 07/05/2022

## Recommendation

EDK2 does not enable AES OCB mode or use the 32-bit AES-NI assembly optimizations.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag

## [CVE-2022-2068 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-2068)

## Published: 06/21/2022

## Recommendation

EDK2 never calls the c_rehash.in script in normal build process.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag

## [CVE-2022-1292 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-1292)

## Published: 05/03/2022

## Recommendation

EDK2 never calls the c_rehash.in script in normal build process.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag

## [CVE-2022-0778 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-0778)

## Published: 03/15/2022

## Recommendation

EDK2 does not use the BN_mod_sqrt() function.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag

## [CVE-2021-4160 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2021-4160)

## Published: 01/28/2022

## Recommendation

The issue only affects MIPS platforms, and we don’t have MIPS in EDK2.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag

## [CVE-2021-3712 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2021-3172)

## Published: 08/24/2021

## Recommendation

Analysis has confirmed that the relevant OpenSSL string operations have been used correctly and safely in EDK2.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag

## [CVE-2021-3711 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2021-3711)

## Published: 08/24/2021

## Recommendation

The SM2 algorithm is not supported by EDK2.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag

## [CVE-2021-3450 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2021-3450)

## Published: 03/25/2021

## Recommendation

 X509_V_FLAG_X509_STRICT is never set by edk2.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag

## [CVE-2021-3449 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2021-3449)

## Published: 03/25/2021

## Recommendation

 Edk2 TLS supports client mode only. This issue only exists in server mode.

 Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag

