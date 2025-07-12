Edwards-Curve Digital Signature Algorithm (EdDSA)
######################################################


ed25519
======================================

`rfc8032 <https://tools.ietf.org/html/rfc8032>`_

`ed25519-High-speed high-security signatures <https://ed25519.cr.yp.to/ed25519-20110705.pdf>`_

`EdDSA for more curves <https://eprint.iacr.org/2015/677>`_

`Curve25519: new Diffie-Hellman speed records <https://www.iacr.org/cryptodb/archive/2006/PKC/3351/3351.pdf>`_

secret EdDSA scalars 是 n+1 bits，c<=n<=b （这里b=256，c=3），n应该足够大，抗kangaroo攻击。注意，最高bit置1，最低的c bits置0。

background
======================================

eddsa secret key b bits, 

H(k)  2*b bits

a 是取H(k)的 3<=i<=b-3 共b-5位， 加i<3的低位全0，加 :math:`2^(b-2)` 对应的b-1位h

A = aB，并压缩表达为A_ 。

:math:`r = H(h_b, ..., h_{2b-1}, M)` ，其中，M为message，:math:`h_b, ..., h_{2b-1}` 为 H(k)的 top b bits。相当于，以k & message派生r。

R = rB，并压缩表达为 R_ 。

.. math::

    S = ( r + H(R_, A_, M)*a ) mod l

签名是(R_, S_)，共2*b bits

验签可以是 8SB = 8R + 8H(R_, A_, M)A

sign bit
==========================================================

如果 b-1 bits 的 x > b-1 bits 的 -x，则，置 x 为 negative

压缩表示 b bits 的 (x , y )： b-1 bits 的 y，加上一个sign bit；如果sign bit为1，则标识 x 为 negative。

r
======================================

r的风险与ecdsa sony playstaion3的案例类似。

eddsa基于k & M 生成r，因此不用随机生成r。

malleability
======================================

已知一个有效签名，可以构造针对相同message的另一个有效签名（取S, A的负值）。

但是无法构造针对不同message的有效签名（因为M参与r, s运算）。

schnorr signature compare
======================================

schnorr signature通过构造(R, S)，并校验SB = R+ H(R_, M)A，避免求逆。

eddsa关键设计为: 2*b bits 的H； A_ 参与 S中的H运算；与schnorr不同的R值处理。

curve addition
======================================

.. math::

  x = X/Z

  y = Y/Z

  coordinates  (X, Y, Z, T) 


fast decompression
======================================

:math:`q = 2^255 - 19` ，mod 8 余 5

用Montgomery变换，避免求逆。

批量校验多个签名
======================================

设 :math:`P_i = 8*R_i + 8*H_i*A_i - 8*S_i*B`

如果每个签名都valid，则，:math:`z_i*P_i` 之和必定为0。

由于 :math:`z_i` 随机，当某个 :math:`P_i` 不为0（即invalid signature），而 :math:`z_i*P_i` 恰好为0时，可能有false positive，但不会false negative。

pureEdDSA
==========================================================

k 为 EdDSA secret key，b bits string

通过 H(k) 转换为 2b bits 的 string，然后拆分计算签名。由于最终签名(R, S)都有消息M的参与，比较抗锻造。

cofactorless verification
==========================================================

默认的校验是等式两边都乘上 cofactor = 2^c 做校验。如果等式两边不乘以cofactor，即为cofactorless verification。

small group attack
==========================================================

基点选得不好，non-prime order，可能被Pohlig-Hellman method加速计算。

