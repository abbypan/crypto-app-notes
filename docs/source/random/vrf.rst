VRF
#####

doc
==========================================================

`Verifiable Random Functions (VRFs) <https://datatracker.ietf.org/doc/draft-irtf-cfrg-vrf>`_


基于签名机制，让verifier校验prover拥有某项内容，同时避免hash遍历的问题、字典攻击。典型例如NSEC3/NSEC5/key transparency。

.. math::

    pi = VRF_prove(SK, alpha)
    beta = VRF_proof_to_hash(pi)
    VRF_hash(SK, alpha) = VRF_proof_to_hash(VRF_prove(SK, alpha))
    (VALID, beta) = VRF_verify(PK, alpha, pi)
    
其实校验还是基于pi做的。


VRF Security Properties
==========================================================

Full uniqueness & Trusted Uniqueness

Full collision resistance & Trusted collision resistance

Full pseudorandomness & Selective pseudorandomness

RSA Full Domain Hash VRF (RSA-FDH-VRF)
==========================================================

思路与RSASSA-PSS类似，参考RFC8017

注意 :math::`beta_string = Hash(two_string || pi_string)`

Elliptic Curve VRF (ECVRF)
==========================================================

思路与EdDSA类似，结合hash_to_curve的基础函数组合处理。

.. math::

    ECVRF_prove:
    基于SK派生scalar x，以及 Y = x*B。
    H = ECVRF_hash_to_curve(Y, alpha_string)
    h_string = point_to_string(H) 
    Gamma = x*H
    k = ECVRF_nonce_generation(SK, h_string)
    c = ECVRF_hash_points(H, Gamma, k*B, k*H)
    s = ( k + c*x ) mod q
    pi_string = point_to_string(Gamma) || int_to_string(c, n) || int_to_string(s, qLen)

    ECVRF_proof_to_hash:
    beta_string = Hash(suite_string || three_string || point_to_string(cofactor * Gamma) || zero_string )

    ECVRF_verify:
    H =  ECVRF_hash_to_curve(Y, alpha_string)
    U = s*B - c*Y
    V = s*H - c*Gamma
    c' = ECVRF_hash_points(H, Gamma, U, V)
    c' == c ?


ECVRF_hash_to_curve
----------------------------------------------------

    ECVRF_hash_to_curve_try_and_increment(Y, alpha_string) 其实就是加一个计数器，参与Y, alpha_string的hash运算，看string_to_point能不能撞到一个valid point。string_to_point的做法参考RFC8032第5.1.3节，string映射到Fp域的x再求解y。

    ECVRF_hash_to_curve_h2c_suite(Y, alpha_string) 复用irtf-cfrg-hash-to-curve的设定


ECVRF_nonce_generation
----------------------------------------------------

   ECVRF_nonce_generation_RFC6979(SK, h_string), 其实就是参考Deterministic ECDSA的做法，基于SK，h_string弄一个PRNG出来

   ECVRF_nonce_generation_RFC8032(SK, h_string), 其实就是参考EdDSA的做法...

   
ECVRF_hash_points
----------------------------------------------------

.. math::

       ECVRF_hash_points(P1, ..., Pm):  
       str = suite_string || two_string || point_to_string(P1) ... point_to_string(Pm) || zero_string
       c_string = Hash(str)
       truncated_c_string = c_string[0 ... n-1]
       c = string_to_int(truncated_c_string)

