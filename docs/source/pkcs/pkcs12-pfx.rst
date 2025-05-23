PKCS #12: Personal Information Exchange Syntax
###################################################

RFC7292 PKCS #12: Personal Information Exchange Syntax

Introduction
==========================================================

PKCS #12 describes a transfer syntax for personal identity information, including private keys, certificates, miscellaneous secrets, and extensions.

Definitions and Notation
==========================================================

Shrouding:  Encryption as applied to private keys

Overview
==========================================================

Exchange Modes
----------------------------------------------------

隐私模式：
- Public-key privacy mode: 信息以接收方公钥加密(源, TPDestEncK)，接收方以自身私钥解密(目标, VDestEncK)
- Password privacy mode: 信息以对称密钥加解密

完整性模式：
- Public-key integrity mode: 信息以发送方私钥签名(源, VSrcSigK)，接收方以发送方公钥验证(目标, TPSrcSigK)
- Password integrity mode: 通过密码参与计算得到Message Authentication Code (MAC)进行验证

两类模式之间可以自由组合，注意 Password privacy mode 跟 Password integrity mode 的password可以不同

The AuthenticatedSafe
----------------------------------------------------

AuthenticatedSafe内容后面可以带签名。
AuthenticatedSafe自身带一系列ContentInfo信息，包含可能被加密的内容(content)。
每个ContentInfo可以带一种类型的内容集合，例如private keys, certificates等等。

MacData参数用于password integrity，可选项。
由password, MacSalt, iterationCount三个参数生成MacKey，注意算法 [Appendix B.  Deriving Keys and IVs from Passwords and Salt](https://tools.ietf.org/html/rfc7292#appendix-B) 已经不推荐了。
MacValue使用authSafe value。

PFX PDU Syntax
==========================================================

PFX
----------------------------------------------------

.. note::

     PFX ::= SEQUENCE {
           version     INTEGER {v3(3)}(v3,...),
           authSafe    ContentInfo,
           macData     MacData OPTIONAL
       }

       MacData ::= SEQUENCE {
           mac         DigestInfo,
           macSalt     OCTET STRING,
           iterations  INTEGER DEFAULT 1
           -- Note: The default is for historical reasons and its
           --       use is deprecated.
       }

AuthenticatedSafe
----------------------------------------------------

.. note::

     AuthenticatedSafe ::= SEQUENCE OF ContentInfo
           -- Data if unencrypted
           -- EncryptedData if password-encrypted
           -- EnvelopedData if public key-encrypted

EncryptedData, EnvelopedData 以 SafeContents 表示


SafeBag
----------------------------------------------------

SafeContents 由 SafeBags 组成

每个SafeBag含有一部分信息，例如一个秘钥、一个证书，等等。以object identifier标识信息类型。

.. note::
   
    SafeContents ::= SEQUENCE OF SafeBag

    SafeBag ::= SEQUENCE {
        bagId          BAG-TYPE.&id ({PKCS12BagSet})
        bagValue       [0] EXPLICIT BAG-TYPE.&Type({PKCS12BagSet}{@bagId}),
        bagAttributes  SET OF PKCS12Attribute OPTIONAL
    }

SafeBag Type
----------------------------------------------------

.. note::

    bagtypes OBJECT IDENTIFIER ::= {pkcs-12 10 1}

    BAG-TYPE ::= TYPE-IDENTIFIER

    keyBag BAG-TYPE ::= {KeyBag IDENTIFIED BY {bagtypes 1}}  #私钥
    pkcs8ShroudedKeyBag BAG-TYPE ::= {PKCS8ShroudedKeyBag IDENTIFIED BY {bagtypes 2}} #加密的私钥 
    certBag BAG-TYPE ::= {CertBag IDENTIFIED BY {bagtypes 3}}  #证书
    crlBag BAG-TYPE ::= {CRLBag IDENTIFIED BY {bagtypes 4}}  #CRL
    secretBag BAG-TYPE ::= {SecretBag IDENTIFIED BY {bagtypes 5}} #用户私密信息
    safeContentsBag BAG-TYPE ::= {SafeContents IDENTIFIED BY {bagtypes 6}} #标识有上述5种类型的多个SafeBag

