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

    pk_i' <- (X_i, pk_i)

    sk_i' <- (x_i, sk_i)


用户的secret key集合（与rights关联）： UP

global public key (MPK), master secret key (MSK) 

.. math::

    MPK <- (tpk, {pk_i'}_i)

    MSK <- (tsk, {sk_i'}_i, UP)


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


