PKCS#7 CMS
##############

RFC2315 PKCS#7: Cryptographic Message Syntax

Definitions
==========================================================

PEM: Internet Privacy-Enhanced Mail, as defined in RFCs 1421-1424

BER: Basic Encoding Rules for ASN.1, as defined in X.690

DER: Distinguished Encoding Rules for ASN.1, as defined in X.690.  DER is a subset of BER.

General syntax
==========================================================

ContentInfo
----------------------------------------------------

.. note::

    ContentInfo ::= SEQUENCE {
         contentType ContentType,
         content
           [0] EXPLICIT ANY DEFINED BY contentType OPTIONAL }

ContentType: data, signedData, envelopedData, signedAndEnvelopedData, digestedData, and encryptedData

对于 signed-data, signed-and-enveloped-data, or digested-data 的数据，会先对DER编码的content作hash计算

对于 enveloped-data or signed-and-enveloped-data 的数据，会根据content-encryption algorithm对内容做加密，写入定长的BER编码content域

SignedData
----------------------------------------------------

.. note::

    SignedData ::= SEQUENCE {
         version Version,
         digestAlgorithms DigestAlgorithmIdentifiers,
         contentInfo ContentInfo,
         certificates
            [0] IMPLICIT ExtendedCertificatesAndCertificates
              OPTIONAL,
         crls
           [1] IMPLICIT CertificateRevocationLists OPTIONAL,
         signerInfos SignerInfos }

SignerInfo
----------------------------------------------------

.. note::

    SignerInfo ::= SEQUENCE {
         version Version,
         issuerAndSerialNumber IssuerAndSerialNumber,
         digestAlgorithm DigestAlgorithmIdentifier,
         authenticatedAttributes
           [0] IMPLICIT Attributes OPTIONAL,
         digestEncryptionAlgorithm
           DigestEncryptionAlgorithmIdentifier,
         encryptedDigest EncryptedDigest,
         unauthenticatedAttributes
           [1] IMPLICIT Attributes OPTIONAL }

DigestInfo
----------------------------------------------------

.. note::

    DigestInfo ::= SEQUENCE {
         digestAlgorithm DigestAlgorithmIdentifier,
         digest Digest }

Enveloped-data content type
==========================================================

EnvelopedData
----------------------------------------------------

.. note::

    EnvelopedData ::= SEQUENCE {
         version Version,
         recipientInfos RecipientInfos,
         encryptedContentInfo EncryptedContentInfo }

       RecipientInfos ::= SET OF RecipientInfo

       EncryptedContentInfo ::= SEQUENCE {
         contentType ContentType,
         contentEncryptionAlgorithm
           ContentEncryptionAlgorithmIdentifier,
         encryptedContent
           [0] IMPLICIT EncryptedContent OPTIONAL }

       EncryptedContent ::= OCTET STRING

RecipientInfo
----------------------------------------------------

encryptedKey 是用recipient's public key加密content-encryption key的结果，确保只有recipient能解密获取content-encryption key

.. note::

    RecipientInfo ::= SEQUENCE {
         version Version,
         issuerAndSerialNumber IssuerAndSerialNumber,
         keyEncryptionAlgorithm

           KeyEncryptionAlgorithmIdentifier,
         encryptedKey EncryptedKey }

       EncryptedKey ::= OCTET STRING

PKCS#7 PADDING
----------------------------------------------------

假设内容长度为l，要求k字节对齐，则：

.. note::

       01 -- if l mod k = k-1
       02 02 -- if l mod k = k-2
       .
       .
       .
       k k ... k k -- if l mod k = 0

Signed-and-enveloped-data content type
==========================================================

SignedAndEnvelopedData
----------------------------------------------------

.. note::

    SignedAndEnvelopedData ::= SEQUENCE {
         version Version,
         recipientInfos RecipientInfos,
         digestAlgorithms DigestAlgorithmIdentifiers,
         encryptedContentInfo EncryptedContentInfo,
         certificates
            [0] IMPLICIT ExtendedCertificatesAndCertificates
              OPTIONAL,
         crls
           [1] IMPLICIT CertificateRevocationLists OPTIONAL,
         signerInfos SignerInfos }

Digested-data content type
==========================================================

DigestedData
----------------------------------------------------

ContentInfo里的content是被计算摘要的content

.. note::

    DigestedData ::= SEQUENCE {
         version Version,
         digestAlgorithm DigestAlgorithmIdentifier,
         contentInfo ContentInfo,
         digest Digest }

       Digest ::= OCTET STRING

Signed-and-enveloped-data content type 过程
==========================================================

随机生成 content-encryption key 

用 recipient's public key 加密 content-encryption key 

加密后的content-encryption key和recipient关联信息，打包到RecipientInfo

根据signer指定的hash算法计算content对应的message-digest

message-digest及其关联信息用signer's private key加密得到res，再用content-encryption key对res做一次加密。注意第二次加密时，res可能要先padding到固定长度

上面二次加密的内容，以及siger的关联信息，打包到SignerInfo

content用content-encryption key加密

前面提到的message-digest算法、RecipientInfo、SignerInfo、encrypted content一起打包到SignedAndEnvelopedData

当recipient收到数据后，先用自身私钥解密获得content-encryption key；然后解密content；然后解密上面二次加密的message-digest；根据解密的content计算message-digest，与解密得到的message-digest比对。
