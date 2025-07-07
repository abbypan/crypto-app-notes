SIGMA 
==========================================================

`SIGMA: the ‘SIGn-and-MAc’ Approach to Authenticated Diffie-Hellman and its Use in the IKE Protocols <https://webee.technion.ac.il/~hugo/sigma-pdf.pdf>`_

`SIGMA: SIGN-and-MAC Crypto rationale and proposals <https://www.ietf.org/proceedings/52/slides/ipsec-9.pdf>`_

basic SIGMA
----------------------------------------------------

通过MAC绑定session与identity

.. math::

    A -> B : g^x

    B -> A : g^y, B, SIG_B (g^x, g^y), MAC_Km(B)

    A -> B : A , SIG_A(g^y, g^x), MAC_Km(A)


SIGMA-I
----------------------------------------------------

保护identity I

变种是 mac -> sig，再结合identity 做 enc

.. math::

    A -> B : g^x

    B -> A : g^y, { B, SIG_B (g^x, g^y), MAC_Km(B) }_Ke

    A -> B : { A , SIG_A(g^y, g^x), MAC_Km(A) }_Ke


SIGMA-R
----------------------------------------------------

保护identity R

变种是 mac -> sig，再结合identity 做 enc

.. math::

    A -> B : g^x

    B -> A : g^y 

    A -> B : { A , SIG_A(g^y, g^x), MAC_Km(A) }_Ke

    B -> A : { B, SIG_B (g^x, g^y), MAC_Km(B) }_Ke


full fledge 
----------------------------------------------------

.. math::

    A -> B : sidA, g^x, nA, info_1_A

    B -> A : sidA, sidB, g^y, nB, info_1_B

    A -> B : sidA, sidB, { info_2_A, A, SIG_A(nB, sidA, g^x, info_1_A, info_2_A), MAC_Km(A) }_Ke

    B -> A : sidA, sidB, { info_2_B, B, SIG_B(nA, sidB, g^y, info_1_B, info_2_B), MAC_Km'(B) }_Ke'
