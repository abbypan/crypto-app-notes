BBS Signature
#################

doc
==========================================================

`The BBS Signature Scheme <https://datatracker.ietf.org/doc/draft-irtf-cfrg-bbs-signatures/>`_

`Zcash Overwinter Consensus and Sapling Cryptography Review <https://research.nccgroup.com/wp-content/uploads/2020/07/NCC_Group_Zcash2018_Public_Report_2019-01-30_v1.3.pdf>`_

`Slide: The BBS Signature Scheme <https://datatracker.ietf.org/meeting/114/materials/slides-114-cfrg-bbs-signature-scheme-pdf-00>`_

overview
==========================================================

bbs的核心特征是short group Signature，支持zkp，选择性披露部分消息(Selective Disclosure)，而proof of possession本身并不泄漏与原始signature的关联(Unlinkable)。

BLS12-381 pairing curve，同zcash，117~120 bits security。

G1/G2均为r质数阶的subgroup，public key在G2，signature在G1。

random要求CSPRNG。

基于IKM，结合keyinfo，派生私钥SK。

PK = P2 * SK, P2为G2的生成元。

header 配置信息，signature/proof中都含header。

messages 内容信息，signature全量，proof按需。

Sign
==========================================================

    Sign(SK, PK, header, messages)

生成确定的generators

.. math::

    (Q_1, Q_2, H_1, ..., H_L) = create_generators(L+2)

基于PK、generators、header计算domain，一个hash2scalar映射。

.. math::

      domain = calculate_domain(PK, Q_1, Q_2, (H_1, ..., H_L), header)

基于SK、domain, msg1 ... msgL 计算 (e, s)，一个expand_message，两个hash2scalar映射。

.. math::

    e_s_octs = serialize((SK, domain, msg_1, ..., msg_L))
    e_s_expand = expand_message(e_s_octs, expand_dst, e_s_len)
    e = hash_to_scalar(e_s_expand[0..(octet_scalar_length - 1)])
    s = hash_to_scalar(e_s_expand[octet_scalar_length..(e_s_len - 1)])

计算A

.. math::

    B = P1 + Q_1 * s + Q_2 * domain + H_1 * msg_1 + ... + H_L * msg_L
    A = B * (1 / (SK + e))

    signature = (A, e, s)

Verify
==========================================================

    ProofVerify(PK, proof, header, ph, disclosed_messages, disclosed_indexes)

同样生成generators、domain

计算B

.. math::

    B = P1 + Q_1 * s + Q_2 * domain + H_1 * msg_1 + ... + H_L * msg_L

校验签名

.. math::

     if e(A, W + P2 * e) * e(B, -P2) != Identity_GT, return INVALID
     return VALID

pairing比较简单，:math:`W = octets_to_pubkey(PK) = P2 * SK`

ProofGen
==========================================================

    proof = ProofGen(PK, signature, header, ph, messages, disclosed_indexes)

L为messages总数，R为披露的messages数，U为未披露的messages数

同样生成generators、domain

生成random列表

.. math::

    random_scalars = calculate_random_scalars(6+U)
    (r1, r2, \tilde{e}, \tilde{r}2, \tilde{r}3, \tilde{s}, \tilde{m}_j1, ..., \tilde{m}_jU) = random_scalars

计算中间参数

.. math::

    B = P1 + Q_1 * s + Q_2 * domain + H_1 * msg_1 + ... + H_L * msg_L
    r3 = r1 ^ -1 mod r
    A' = A * r1
    Abar = A' * (-e) + B * r1
    D = B * r1 + Q_1 * r2
    s' = r2 * r3 + s mod r
    C1 = A' * \tilde{e} + Q_1 * \tilde{r}2
    C2 = D * (-\tilde{r}3) + Q_1 * \tilde{s} + H_j1 * \tilde{m}_j1 + ... + H_jU * \tilde{m}_jU

计算challenge，同样是hash2scalar

.. math::

    c = calculate_challenge(A', Abar, D, C1, C2, (i1, ..., iR), (msg_{i1}, ..., msg_{iR}), domain, ph)

计算proof

.. math::

    \hat{e} = c * e + \tilde{e} mod r
    \hat{r}2 = c * r2 + \tilde{r}2 mod r
    \hat{r}3 = c * r3 + \tilde{r}3 mod r
    \hat{s} = c * s' + \tilde{s} mod r
    for j in (j1, ..., jU): \hat{m}_j = c * msg_j + \tilde{m}_j mod r
    proof = (A', Abar, D, c, \hat{e}, \hat{r}2, \hat{r}3, \hat{s}, (\hat{m}_j1, ..., \hat{m}_jU))

注意 :math:`j_1, ..., j_U` 是未披露的messages index

ProofVerify
==========================================================

    result = ProofVerify(PK, proof, header, ph, disclosed_messages, disclosed_indexes)

同样生成generators、domain

计算C1

.. math::

    C1 = (Abar - D) * c + A' * \hat{e} + Q_1 * \hat{r}2
       = (A' * (-e) + B * r1 - D) * c + A' * \hat{e} + Q_1 * \hat{r}2
       = (A' * (-e) - Q_1 * r2) * c + A' * \hat{e} + Q_1 * \hat{r}2
       = A' * (\hat{e} - e * c) + Q_1 * (\hat{r}2 - r2 * c)
       = A' * \tilde{e} + Q_1 * \tilde{r}2
       = C1

计算C2

.. math::

    T = P1 + Q_2 * domain + H_{i1} * msg_{i1} + ... + H_{iR} * msg_{iR}

    C2 = T * c - D * \hat{r}3 + Q_1 * \hat{s} + H_{j1} * \hat{m}_{j1} + ... + H_{jU} * \hat{m}_{jU}
       = (P1 + Q_2 * domain + H_{i1} * msg_{i1} + ... + H_{iR} * msg_{iR}) * c - D * (c * r3 + \tilde{r}3) + Q_1 * (c * s' + \tilde{s})  + H_{j1} * (c * msg_{j1} + \tilde{m}_{j1}) + ... + H_{jU} * (c * msg_{jU} + \tilde{m}_{jU})
       = (P1 + Q_2 * domain + H_1 * msg_1 + ... + H_L * msg_L - D * r3 + Q_1 * s') * c - D * \tilde{r}3 + Q_1 * \tilde{s} + H_{j1} * \tilde{m}_{j1} + ... + H_{jU} * \tilde{m}_{jU} 
       = (B - Q_1 * s - D * r3 + Q_1 * s') * c + D * (-\tilde{r}3) + Q_1 * \tilde{s} + H_{j1} * \tilde{m}_{j1} + ... + H_{jU} * \tilde{m}_{jU} 
       = (B + Q_1 * r2 * r3 - B * r1 * r3 - Q_1 * r2 * r3) * c + D * (-\tilde{r}3) + Q_1 * \tilde{s} + H_{j1} * \tilde{m}_{j1} + ... + H_{jU} * \tilde{m}_{jU} 
       = D * (-\tilde{r}3) + Q_1 * \tilde{s} + H_{j1} * \tilde{m}_{j1} + ... + H_{jU} * \tilde{m}_{jU}   
       = C2

计算cv 

.. math::

    cv = calculate_challenge(A', Abar, D, C1, C2, (i1, ..., iR), (msg_{i1}, ..., msg_{iR}), domain, ph)

检查cv是否与c相等

.. math::

    if A' == Identity_G1, return INVALID
    if e(A', W) * e(Abar, -P2) != Identity_GT, return INVALID

    e(A', W) * e(Abar, -P2)
    = e(A', P2 * SK) * e(A' * (-e) + B * r1, -P2)
    = e(A * r1 * SK, P2) * e( (A * (-e) + B) * r1, -P2)
    = e(A * r1 * SK, P2) * e( A * SK * r1, -P2)
    = Identity_GT

security
==========================================================

valid public key

valid point

prime order check

run in constant time

nonce reuse attack

header中带个nonce

G1与G2不同构

DRBG

proof replay attack

use case
==========================================================

改进oauth2式的bearer access token

改进oauth2 DPoP式的校验形态，不用hmac/hash啥的

verifiable credential，例如driver license

