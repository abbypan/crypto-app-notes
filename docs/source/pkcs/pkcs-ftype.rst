PKCS file type
##################

`ASN.1 introduction <https://www.itu.int/en/ITU-T/asn1/Pages/introduction.aspx>`_

pem, cer, crt, der, p7b, p7c, p12, pfx

`Certificate filename extensions <https://en.wikipedia.org/wiki/X.509>`_

`What are the differences between PEM, DER, P7B/PKCS#7, PFX/PKCS#12 certificates <https://myonlineusb.wordpress.com/2011/06/19/what-are-the-differences-between-pem-der-p7bpkcs7-pfxpkcs12-certificates/>`_

证书
==========================================================

.pem – 证书 (Privacy-enhanced Electronic Mail) Base64 encoded DER certificate, enclosed between "-----BEGIN CERTIFICATE-----" and "-----END CERTIFICATE-----"

.cer, .crt, .der – binary DER编码的证书，或者Base64 DER编码(pem兼容)

.p7b, .p7c – PKCS#7 SignedData structure without data, just certificate(s) or CRL(s)

信息交换
==========================================================

.p12 – PKCS#12, may contain certificate(s) (public) and private keys (password protected)

.pfx – PFX personal information exchange, predecessor of PKCS#12 (usually contains data in PKCS#12 format, e.g., with PFX files generated in IIS)

openssl operation
==========================================================

`The Most Common OpenSSL Commands <https://www.sslshopper.com/article-most-common-openssl-commands.html>`_

`openssl command <https://gist.github.com/webtobesocial/5313b0d7abc25e06c2d78f8b767d4bc3>`_

`How to Convert certificates between PEM, DER, P7B/PKCS#7, PFX/PKCS#12 <https://myonlineusb.wordpress.com/2011/06/19/how-to-convert-certificates-between-pem-der-p7bpkcs7-pfxpkcs12/>`_




