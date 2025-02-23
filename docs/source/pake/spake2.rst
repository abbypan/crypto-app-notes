SPAKE2
==========================================================

`Simple Password-Based Encrypted Key Exchange Protocols <https://www.di.ens.fr/david.pointcheval/Documents/Papers/2005_rsa.pdf>`_
的SPAKE2是 2-message 交互，通过M，N的password幂计算协作完成类DH交换，wPFS 

`Forward Secrecy of SPAKE2 <https://eprint.iacr.org/2019/351.pdf>`_
的PFS-SPAKE2是 3-message 交互，通过M的password幂计算协作完成类DH交换，加入了两个中间的hash确认码，PFS

通信双方已知基点P，以及另外两个点M & N，且分别选取一个随机数x
    
.. math::
        A -> B : x_a * P + pw * M
        B -> A : x_b * P + pw * N
        显然，最终双方能获得 x_a*x_b*P
