SESPAKE
==========================================================

RFC8133

通信双方已知基点P，基于P以指定的函数派生`{ Q_1, ..., Q_N }`，且分别选取一个随机数x

.. math::

    B -> A : 随机的ind、salt
    A 计算 : Q_PW = int(F(PW, salt, 2000))*Q_ind
    A -> B : u_1 = x_a*P - Q_PW
    B -> A : u_2 = x_b*P + Q_PW
    显然，最终双方能获得 x_a*x_b*P

如果B是服务端，那么可以存储{ ind, salt, Q_PW }，避免明文存储PW再实时计算Q_PW => 此时，则为augumented模式。
