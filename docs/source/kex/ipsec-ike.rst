IPsec/IKE
#############

- `BSI TR-02102 Cryptographic Mechanisms <https://www.bsi.bund.de/EN/Service-Navi/Publications/TechnicalGuidelines/tr02102/tr02102_node.html>`_
- `RFC7427: Signature Authentication in the Internet Key Exchange Version 2 (IKEv2) <https://tools.ietf.org/html/rfc7427>`_
- `RFC7296: Internet Key Exchange Protocol Version 2 (IKEv2) <https://tools.ietf.org/html/rfc7296>`_


主要是SA (security association)
- DH 派生 z
- 派生SKEYSEED = prf( Ni | Nr, z )，其中Ni/Nr 为双方的随机数
- :math:`prf+(SKEYSEED, Ni | Nr | SPIi | SPIr ) = { SK_d | SK_ai | SK_ar | SK_ei | SK_er | SK_pi | SK_pr }` ，其中SPIi/SPIr为双方的唯一标识

    SK_d用于Child-SAs派生key
    SK_ei/SK_er用于加密
    SK_ai/SK_ar用于完整性
    SK_pi/SK_pr用于IKE_AUTH exchange消息的AUTH payload的完整性

注意重新DH协商的周期，rekeying的周期
