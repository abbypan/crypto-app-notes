PKES
==============================================

`Public Key Encryption with keyword Search <https://crypto.stanford.edu/~dabo/pubs/papers/encsearch.pdf>`_

基于pairing构造trapdoor。

KeyGen
-------------

.. math::
        random α ∈ Z∗

        generator g of G_1 

        A_{pub} = [g, h = g^α]
        
        A_{priv} = α

PKES
-------------------

W 为关键字，输出关键字密文 S

.. math::
        PEKS(A_{pub}, W):

                random r ∈ Z∗

                t = e(H_{1}(W), h^r) ∈  G_2

                S = [ g^r , H_{2}(t) ]

                return S

Trapdoor
-----------

W 为待查的关键字

.. math::

        Trapdoor(A_{priv} , W ): 
        
                T_W = H_{1}(W)^α ∈ G1.


针对关键字密文执行查询
-------------------------

关键字密文为 S

待查的关键字陷门为 T_W

.. math::

        Test(A_{pub}, S, T_W ): 
        
                [A, B] = S
                
                if H_{2}(e(T_W , A)) == B  : 

                      return yes

                return no
