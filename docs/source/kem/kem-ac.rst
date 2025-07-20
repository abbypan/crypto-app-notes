KEMs with Access Control (KEMAC)
===================================

`ETSI TS 104 015 Cyber Security (CYBER); Quantum-Safe Cryptography (QSC); Efficient Quantum-Safe Hybrid Key Exchanges with Hidden Access Policies <https://www.etsi.org/deliver/etsi_ts/104000_104099/104015/01.01.01_60/ts_104015v010101p.pdf>`_

本质是 CP-ABE

Hybrid Traceable KEMAC 核心是 key policy 的保护，尝试 (i, j) 遍历以解密出session key K

rights 采用 DNF 映射 

ML-KEM
----------

基于pqc的kem，负责encap，decap。


NIKE-based KEM
----------------

传统ecc的dh，生成session key。


setup
-----------

随机生成NIKE专用的密钥对 (H, s), (P1, s1), (P2, s2) , P为generator

用户标识集为ID

tracing secret key (tsk) :  (s, s1, s2, ID)

tracing public key (tpk) : (P, H, P1, P2)


rights
--------------

权限集合为

.. math::

    \Omega = { S_1, ..., S_n }


为每个 :math:`S_i` 生成 

.. math::

    (pk_i, sk_i) <- KEM.keygen

    (X_i, x_i) <- NIKE.keygen

    H_i <- NIKE.sessionKey(s, X_i)  

    pk'_i <- (X_i, pk_i)

    sk'_i <- (x_i, sk_i)


用户的secret key集合（与rights关联）： UP

global public key (MPK), master secret key (MSK) 

.. math::

    MPK <- (tpk, {pk'_i}_i)

    MSK <- (tsk, {sk'_i}_i, UP)


HTKEMAC.KeyGen(MSK, U, Y) -> (USK, MSK', tsk')
--------------------------------------------------

用户U的rights集合为Y

为用户U生成secret identifier : uid

.. math::

    \alpha <- random

    \beta = (s - \alpha * s_1 ) / s_2 mod p

    s = \beta * s_2 + \alpha * s_1

    H = \alpha * P1 + \beta * P2

    uid <- ( \alpha, \beta )

    将 (U, uid) 更新到 tsk 的 ID内，获得 tsk'。

    USK <-  (uid, {sk'_j}_{j \in Y})

    将USK添加到MSK的UP内，获得 MSK'。

    (USK, MSK', tsk')

HTKEMAC.Enc(MPK, X) →  (C, K)
---------------------------------

指定的rights集合为X

随机生成S, 计算 

.. math::

    r <- G(S)

    c_1 <- r * P_1

    c_2 <- r * P_2

    c <- (c_1, c_2)


对X中的每个i，计算

.. math::

    K_i <- NIKE.SessionKey(r, H_i)

    (E_i, K'_i) <- KEM.ENC(pk_i)

    F_i <- S xor H(K_i, K'_i, c, {E_l}_{l \in X})


最终

.. math::

    (K, V) <- J(S, c, {E_i, F_i}_{i \in X})

    C <- (c, {E_i, F_i}_{i \in X}, V)


HTKEMAC.Dec(USK, C) → K
------------------------------

对X中的每个i，对于Y中的每个j，计算

.. math::

    K'_{i,j} <- KEM.Dec(sk_j, E_i)

    K_j <- NIKE. SessionKey(x_j, \alpha * c_1 + \beta * c_2)

    S_{i, j} <- F_i xor H(K_j, K'_{i, j}, c, {E_l}_{l \in X})

    r' <- G(S_{i, j})

    (U'_{i, j}, V'_{i, j}) <- J(S_{i, j}, c, {E_i, F_i}_{i \in X})

    检查 c 是否与 (r' * P_1, r' * P_2) 相符

    检查 V'_{i, j} 是否与 V 相符

    如果两者相符，则返回 K <- U'_{i, j}；否则，继续尝试下一个 { i, j }


Access structure
-------------------

.. math::

    semantic space: sem(\Omega, r) 相当于 r 的展开

    complementary space: comp(\Omega, r) = \{ P_{r'}, r' \le r \} + (\Omega \backslash  sem(\Omega, r))  相当于 小于等于r的rights 结合 非sem(\Omega, r)的集合

    示例 CTR::FR \&\& SEC::MED，则

    r = (2, 0, 2)

    { P_{r'}, r' \le r } = { (2, 0, 1), (2, 0, 2) }

    (\Omega \backslash  sem(\Omega, r)) = < (0, 1, 0) >

    comp(\Omega, r) = \{ (2, 0, 1), (2, 1, 1), (2,2, 1), (2, 0, 2), (2, 1, 2), (2, 2, 2) \}


对于每个encapsulation，其access policy所关联的rights个数，即为其覆盖的clauses数

每个user secret key所关联的rights数，即该user所拥有的rights对应的comp空间

