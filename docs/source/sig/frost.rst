FROST Threshold Signatures
#############################

# FROST

`FROST: Flexible Round-Optimized Schnorr Threshold Signatures <https://eprint.iacr.org/2020/852.pdf>`_

threshold signature 

基于schnorr signature, 注意 :math:`c=H(R, Y, m)`

Feldman’s Verifiable Secret Sharing (VSS) Scheme
==========================================================

t-1阶多项式f, :math:`f(0)=s` ，coffients为 :math:`(a_1, . . . , a_{t−1})` 

distributing the private share (i, f (i)) to each participant Pi

.. math::

    C = < φ_0, . . . , φ_{t−1} >
    φ_0 = g^s
    φ_j = g^{a_j}

显然，可以用Lagrange校验

KeyGen
==========================================================

基于VSS。

Round 1: 

每个Pi都随机生成自己的 :math:`f_i`, 以 :math:`a_{i0}` 为私钥计算 `φ_{i0}=g^{a_{i0}}` 的schnorr signature :math:`σ_i` , 广播 :math:`σ_i` 、:math:`C_i = < φ_{i0}, . . . , φ_{i(t−1)} >` 。

Round 2: 

Each :math:`P_i` securely sends to each other participant :math:`P_l` a secret share :math:`(l, f_i(l))`

:math:`P_i`  校验 :math:`g^{f_l(i)}` 等于 :math:`φ_{lk}^(i^k mod q), 0 ≤ k ≤ t-1` 的积，相当于 :math:`P_i` 确认 :math:`f_i(l)` 与 :math:`P_l` 发布的 :math:`C_l` 匹配

:math:`P_i`  计算

.. math::

    s_i = ∑ f_l(i), 1 ≤ l ≤ n
    Y_i = g^{si}
    Y = ∏ φ_{j0}, 1 ≤ j ≤ n

其他 :math:`P_j` 可以校验 :math:`Y_i = ∏ ∏ φ_{lk}^(i^k mod q), 0 ≤ k ≤ t-1; 1 ≤ l ≤ n`

 :math:`s_i` 相当于每个Participants的f在i上取值的和，作为 :math:`P_i` 的私钥

而Y相当于以 **每个Participants的f的0上取值的和** 求幂, 即为公共Public Key

Preprocess for signing
==========================================================

每个 :math:`P_i` 随机生成nonce list  :math:`((d_{ij} , D_{ij}), (e_{ij}, E_{ij})), 1 ≤ j ≤ π`,  :math:`L_i` 是 :math:`(D_{ij}, E_{ij}), 1 ≤ j ≤ π` 的集合

一次Preprocess生成的π份nonce，可以算π次signature

用过即删


Signing
==========================================================

SA为此次Signing选择 :math:`α : t ≤ α ≤ n` participants， the next available commitment :math:`(D_i, E_i) : i ∈ S` 集合记为B。

.. math::

    ρ_i = H1(i, m, B)

    k_i = d_i + e_i · ρ_i

    R_i = D_i · E_i^(ρ_i)
    z_i = d_i + (e_i · ρ_i) + λ_i · s_i · c

    verify g^(z_i) = R_i · Y_i^(c·λ_i)

    R = ∏ R_i, i∈S 
    z = ∑ z_i, i∈S 
    σ = (R, z)

    verify R = g^z · Y^(-c)

显然，secret nonce相当于 :math:`k = ∑ k_i , i∈S`

FROST-Interactive
==========================================================

主要是 :math:`ρ_i` 的区别

Preprocess
----------------------------------------------------

:math:`(i, 〈(D_{ij} , E_{ij} , A_{ij} , B_{ij} )〉, 1 ≤ j ≤  π)`

注意 :math:`A_{ij}, B_{ij}` 用于辅助校验 :math:`ρ_i`

Signing
----------------------------------------------------

SA公开所有 :math:`ρ_i`

.. math::

    ρ_i = a_{ij} + b_{ij} · Hρ(m, B)

每个Participant校验

.. math::

    g^{ρ_i} = = A_{ij} · B_{ij}^(Hρ(m,B))

two round
==========================================================

`Two-Round Threshold Schnorr Signatures with FROST <https://datatracker.ietf.org/doc/draft-irtf-cfrg-frost/>`_

参数名贼长。。。

.. math::

    hiding_nonce: d_i
    hiding_nonce_commitment: D_i

    binding_nonce: e_i
    binding_nonce_commitment_i: E_i

    binding_factor: ρ_i
