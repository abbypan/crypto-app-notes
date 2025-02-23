OPAQUE
==========================================================

`OPAQUE: An Asymmetric PAKE ProtocolSecure Against Pre-Computation Attacks <https://eprint.iacr.org/2018/163.pdf>`_

.. math::

    服务端S生成公私钥对 { Priv_U, Pub_U }, { Priv_S, Pub_S }

    S随机生成K_s
    客户端U与服务端S会计算共享密钥：RW=OPRF(K_s, pw) =H(pw,(H'(pw))^K_s)

    注意，这里S不会把K_s明文传给U，而是通过 

        U随机生成r, x_u
        U -> S: a = H'(pw)^ r , X_u = g^x_u
        S -> U: b = a^K_s
        显然，最终U与S能获得RW

    S使用RW封装c = AuthEnc_rw({ Priv_U, Pub_U, Pub_S })
    S为U保存 { K_s, Priv_S, Pub_S, Pub_U, c }

    S生成随机数x_s，以及对应的公钥X_s
    S计算K = KE(Priv_S, x_s, Pub_U, X_u),   ssid' = H(sid, ssid, a), SK = f_K(0, ssid'), A_s = f_K(1, ssid'), A_u = f_K(2, ssid')
    S -> U : b, X_s, c, A_s
    
    U解密c，获得Priv_U, Pub_U, Pub_S
    U计算 K=KE(Priv_U, x_u, Pub_S, X_s)，同样计算ssid', SK, A_s, A_u

    其中KE为HMQV计算:
    S: KE = H((X_u * Pub_U^e_u)^(x_s + e_s*Priv_S))
    U: KE = H((X_s * Pub_S^e_s)^(x_u + e_u*Priv_U))
    e_u = H(X_u, S, ssid'), e_s = H(X_s, U, ssid')
