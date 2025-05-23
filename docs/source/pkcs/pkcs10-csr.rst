PKCS #10 CSR
##################

RFC2986 PKCS #10 Certification Request Syntax Specification

Introduction 
==========================================================

certification request中的关键信息：distinguished name, public key, attributes (optional)，以及针对上述内容的signature（由请求方使用该public key对应的private key计算）。

certification request包含3部分：certification request information，签名算法标识，signature

Certification Request Infomation包含：subject distinguished name, subject's public key, attributes (optional)

CertificationRequestInfo的内容用DER编码成octet string，然后使用subject's private key对其进行签名，得到signature（BIT STRING）。

CA签发证书之前，必须校验CSR中的签名。


CertificationRequestInfo
==========================================================

.. note::

    CertificationRequestInfo ::= SEQUENCE {
            version       INTEGER { v1(0) } (v1,...),
            subject       Name,
            subjectPKInfo SubjectPublicKeyInfo{{ PKInfoAlgorithms }},
            attributes    [0] Attributes{{ CRIAttributes }}
       }

       SubjectPublicKeyInfo { ALGORITHM : IOSet} ::= SEQUENCE {
            algorithm        AlgorithmIdentifier {{IOSet}},
            subjectPublicKey BIT STRING
       }

       PKInfoAlgorithms ALGORITHM ::= {
            ...  -- add any locally defined algorithms here -- }

       Attributes { ATTRIBUTE:IOSet } ::= SET OF Attribute{{ IOSet }}

       CRIAttributes  ATTRIBUTE  ::= {
            ... -- add any locally defined attributes here -- }

       Attribute { ATTRIBUTE:IOSet } ::= SEQUENCE {
            type   ATTRIBUTE.&id({IOSet}),
            values SET SIZE(1..MAX) OF ATTRIBUTE.&Type({IOSet}{@type})
       }
