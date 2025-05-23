Direct Anonymous Attestation
##################################

doc
==========================================================

`Direct Anonymous Attestation in the Wild <https://rwc.iacr.org/2019/slides/DAA.pdf>`_

`Direct Anonymous Attestationbased on Elliptic Curve CryptographyA feasibility Study for RFID <https://diglib.tugraz.at/download.php?id=576a88247699a&location=browse>`_

`A Symbolic Analysis of ECC-based DirectAnonymous Attestation <https://people.inf.ethz.ch/rsasse/pub/eccdaa-eurosp19.pdf>`_

correctness, unforgeability, unclonability, unlinkability (full anonymity), revokability, practicability


mobile
==========================================================

`Lightweight Anonymous Authentication with TLS and DAA for Embedded Mobile Devices <https://eprint.iacr.org/2011/101.pdf>`_

通过hsm避免credential copy
- issuer在工厂向mobile device的secure element(例如tpm)写入secret f，host存储secret对应的cred

校验：
- verifier 发送一个挑战码N；
- mobile device的host生成一个随机数t，将cred映射为cred'; 并将N, cred'等相关信息发给mobile device的secure element
- mobile device的secure element基于之前存储的secret值，结合收到的信息，计算签名。
- mobile device的host将签名、cred'返回给verifier校验

底层运算细节基于pairing curve

pba
==========================================================

`Property-Based Attestation without a Trusted Third Party <https://www.researchgate.net/publication/220905214_Property-Based_Attestation_without_a_Trusted_Third_Party>`_

基于 ring signature，校验是否满足某一属性

tpm仅计算一个signature，host伪装构造出y1, ..., yn

rats
==========================================================

`Direct Anonymous Attestation for the Remote Attestation Procedures Architecture <https://datatracker.ietf.org/doc/draft-ietf-rats-daa/>`_

DAA Issuer 为 Attester 签发 credential

Attester 通过anonymous attestation，向 Verifier 提交证明

由于anonymous attestation已随机化，与credential之间无关，不论DAA Issuer/Verifier都对Attester unlinkable

v2x
==========================================================

`Privacy-enhanced capabilities for VANETs using direct anonymous attestation <https://www.semanticscholar.org/paper/Privacy-enhanced-capabilities-for-VANETs-using-Whitefield-Chen/37eadba8a4d6e4e1bd3efefb73adb98154fbf634), [slide](https://jwhitefield.co.uk/assets/res/talks/VNC17.pdf>`_

`Securing V2X Communications for the Future: Can PKI Systems offer the answer? <https://www.researchgate.net/publication/335089342_Securing_V2X_Communications_for_the_Future_Can_PKI_Systems_offer_the_answer>`_

fido
==========================================================

`fido common specs <https://fidoalliance.org/specs/common-specs/>`_
