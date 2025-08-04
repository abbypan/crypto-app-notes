SAS
=====

Short Authenticated Strings

Three round mutual authentication protocol MA-3

f: enc, h: hmac

.. math::

        Alice :  (c, d) <- Com_{pk}(r_a, s),  c = hash(r_a, s)

        Alice -> Bob:  (m_a, c)

        Bob -> Alice: (r_b, m_b)

        Alice -> Bob: d = (r_a, s)

        Alice:  k = f(r_a, r_b), check_a = h(m_a || m_b, k) 

        Bob:  c == hash(d) ? ,  k = f(r_a, r_b), check_b = h(m_a || m_b, k) 

        Alice <-> Bob: check_a == check_b ?, accept m_a || m_b


doc
---------

- `Secure Communications over Insecure Channels Based on Short Authenticated Strings <https://www.iacr.org/archive/crypto2005/36210303/36210303.pdf>`_
- `Efficient Mutual Data Authentication Using Manually Authenticated Strings <https://eprint.iacr.org/2005/424.pdf>`_
- `sas <https://github.com/raphting/sas/blob/main/main.go>`_
- `SAS-based Authenticated Key Agreement <https://secu.famillepasini.ch/files/publications/PasiniVaudenay06-SASbasedAKA_slides.pdf>`_
- `SAS-Based Group Authentication and Key Agreement Protocols <https://www.iacr.org/archive/pkc2008/49390198/49390198.pdf>`_
- `An Analysis of End-to-End Encryption and Authentication Ceremonies in Secure Messaging Systems <https://dl.acm.org/doi/pdf/10.1145/3558482.3581773>`_

