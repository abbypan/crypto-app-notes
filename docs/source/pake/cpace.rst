CPace
==========================================================

`cpace <https://datatracker.ietf.org/doc/draft-irtf-cfrg-cpace/>`_

复用hash_to_curve的映射，基于PRS和相关参数，在选定的曲线上映射到某个生成元G: G.calculate_generator(H,PRS,CI,sid)
- H: hash_to_field的hash算法
- PRS: 为password的派生值、或者password本身
- CI: channel标识
- sid: session标识

A/B双方各自生成一个1 ~ n-1的随机数ya/yb，对应的Point为 :math::`Ya = G * ya, Yb = G * Yb`
A/B各自的身份标识为ADa/ADb
A/B互相交换[Ya, ADa], [Yb, ADb]

A/B结合所交换的信息，各自派生ISK
- K = G.scalar_mult_vfy(yb,Ya) = G.scalar_mult_vfy(ya, Yb)
-  order 如果A/B按initiator/responder的顺序交互信息：ISK = H.hash(prefix_free_cat(G.DSI || "_ISK", sid, K)||MSGa || MSGb)
-  unorder 如果 A/B 并行交互信息：ISK = H.hash(prefix_free_cat(G.DSI || "_ISK", sid, K)||ocat(MSGa, MSGb)) ，其中ocat是对MSGa/MSGb做排序后的拼接。
-  显然，派生参数包含了双方所有交互信息

cpace整个协议流比较简洁，漂亮。

