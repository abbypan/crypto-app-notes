use RSA-KEM in CMS
#######################

`RFC 9690 Use of the RSA-KEM Algorithm in the Cryptographic Message Syntax (CMS) <https://www.rfc-editor.org/rfc/rfc9690.html>`_

`ISO/IEC 18033-2 Information technology — Security techniques — Encryption algorithms Part 2: Asymmetric ciphers <https://www.iso.org/standard/37971.html>`_


直接用ISO/IEC 18033-2的RSA-KEM加密随机数z，通过KDF派生KEK。KEK保护CEK。CEK保护Private Key。

.. math::

       ct = z^e mod n
       SS = KDF(z, ssLen)
       KEK = KDF(SS, kekLength, otherInfo)
       WK = WRAP(KEK, CEK)
