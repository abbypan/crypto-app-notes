SRP-6a
##########################################################

RFC2945

`A Comparison of the Password-Authenticated Key Exchange Protocols, SRP-6a and PAKE2+ <http://www.diva-portal.org/smash/get/diva2:1354154/FULLTEXT01.pdf>`_


.. math::

    k是双方都知道的整数值
    
    U : salt = s,  x = hash(pwd, s),  v = g^x
    S记录了与U关联的salt = s, v

    U生成随机数a
    U -> S:  A = g^a,  Username
    
    S根据Username取出对应的s, v
    S生成随机数b
    S计算： B = k*v + g^b
            u = hash(A, B)
            S = ( A * v^u )^b
            K = hash(S)
    S -> U : s, B

    U计算： u = hash(A, B)
            x = hash(pwd, s)
            S = ( B - k*g^x )^(a + u*x)
              = ( k*v + g^b - k*v )^(a + u*x)
              = g^(b * (a + u*x))
              =  (g^a * g^(x*u) )^b
              =  (A * v^u)^b
            K = hash(S)
            M1 = hash(A, B, K)
            
