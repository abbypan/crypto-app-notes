hkdf
======================================

`RFC 5869: HMAC-based Extract-and-Expand Key Derivation Function (HKDF) <https://tools.ietf.org/html/rfc5869>`_

.. raw::

   HKDF-Extract(salt, IKM) -> PRK
        salt 如果不指定，则默认为hash len的全0 string
        IKM为初始化key
        PRK = HMAC-Hash(salt, IKM)
    
   HKDF-Expand(PRK, info, L) -> OKM 
        info为相关初始化信息标识
        L为目标OKM的长度

        N = ceil(L/HashLen)
        T = T(1) | T(2) | T(3) | ... | T(N)
        OKM = first L octets of T

        T(0) = empty string (zero length)
        T(1) = HMAC-Hash(PRK, T(0) | info | 0x01)
        T(2) = HMAC-Hash(PRK, T(1) | info | 0x02)
        ...


