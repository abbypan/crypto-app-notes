BBS Signature
#################

`Slide: The BBS Signature Scheme <https://datatracker.ietf.org/meeting/114/materials/slides-114-cfrg-bbs-signature-scheme-pdf-00>`_

https://datatracker.ietf.org/doc/draft-irtf-cfrg-bbs-signatures/

https://datatracker.ietf.org/doc/draft-irtf-cfrg-bbs-blind-signatures/

https://github.com/microsoft/bbs-node-reference

https://github.com/mattrglobal/pairing_crypto


use case
==========================================================

改进oauth2式的bearer access token

改进oauth2 DPoP式的校验形态

verifiable credential，例如driver license, selective disclosure

overview
==========================================================

bbs的核心特征是short signature，支持zkp，选择性披露部分消息(Selective Disclosure)，而proof of possession本身并不泄漏与原始signature的关联(Unlinkable)。

BLS12-381 pairing curve，同zcash，117~120 bits security。

G1/G2均为r质数阶的subgroup，public key在G2，signature在G1。

G1/G2 的base point 为 BP1/BP2。

random要求CSPRNG。

PK = BP2 * SK, P2为G2的生成元。

CoreSign
==========================================================

.. math::

    CoreSign(SK, PK, generators, header, messages, api_id)

生成确定的generators

.. math::

    (Q_1, H_1, ..., H_L) = createGenerators(L+1, apiId)

基于PK、generators、header计算domain，一个hash2scalar映射。

.. math::

      domain = calculateDomain(PK, Q_1, (H_1, ..., H_L), header, apiId)

基于SK、domain, msg1 ... msgL 计算 e

.. math::

    e = hash2scalar(serialize((SK, msg_1, ..., msg_L, domain)), hash_to_scalar_dst)

计算A

P1 为 G1 内的常数point，可以为BP1或其他point

.. math::

    B = P1 + Q_1 * domain + H_1 * msg_1 + ... + H_L * msg_L

    A = B * (1 / (SK + e))

    signature = (A, e)

CoreVerify
==========================================================

.. math::

    CoreVerify(PK, signature, generators, header, messages, api_id)

同样生成generators、domain

计算B

.. math::

    B = P1 + Q_1 * domain + H_1 * msg_1 + ... + H_L * msg_L

校验签名

.. math::

     W = octetsToPubkey(PK) = BP2 * SK

     h(A, W) * h(A * e - B, BP2) = Identity_{GT}


ProofGen
==========================================================

实现 selective disclosure

ph
----

注意以 present_header (ph) 区分不同 proof

ph 由prover给出，必须 随机、区隔 dst_domain，防钓鱼

或者ph由直接由verifier给出


c
----

构造challenge c

        proof_init_output = (Abar, Bbar, D, T1, T2, domain)

        c=H(proof_init_output ∥ disclosed messages ∥ disclosed indexes ∥ ph ∥ other context)

hide
--------

区分 hide attributes, disclosure attributes

对于每个hide attribute  m_j，通过challenge c，随机数 m'_j，构造 m~_j

.. math::

        m~_j = m'_j + m_j * c


ProofVerify
==========================================================

pairing


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


blind signature
===================

增加一个 s_user 的hide attr，受secure element保护, non-transferability

issuer 仅知晓 C = s_user * H0，不知晓 s_user，blind issuance

proof时，s_user总是隐藏


