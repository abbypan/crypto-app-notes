qDSA
=====

qID 与 basic Schnorr identification protocol 的重要区别：

- Scalar multiplication : 可以乘飞
- Verification on K: 模糊校验
- Challenge from Zn+: 限定一下c为正数

DH & signature 可以用完全相同的key pair表示，不用转来转去。


参考资料
---------

- `qDSA: Small and Secure Digital Signatures with Curve-based Diffie–Hellman Key Pairs <https://eprint.iacr.org/2017/518.pdf>`_
