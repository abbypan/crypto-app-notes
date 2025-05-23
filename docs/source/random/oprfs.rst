oprfs
#######

`Oblivious Pseudorandom Functions (OPRFs) using Prime-Order Groups <https://datatracker.ietf.org/doc/draft-irtf-cfrg-voprf>`_

PRF(pseudorandom function): F(k, x) 输出与随机数是indistinguishable

OPRF(oblivious pseudorandom function): client知道x，F(k, x)，但不知道k；server 协助计算F(k, x)，但不知道x, F(k, x)。

Verifiable OPRF(VOPRF): server 可以向 client 证明 F(k, x) 由 k 计算得到

Partially-Oblivious PRF(POPRF): F(k, x, info)，其中info是client/sever双方都知道的内容，其余逻辑与VOPRF类似

NIZK: non-interactive zero knowledge

OPRF/VOPRF/POPRF用于 pake, privacy pass protocol 等场景。

Preliminaries
==========================================================

选取了质数阶的群。

函数定义: k*A = B, k*C = D
- 证明方： proof = (c, s) = GenerateProof(k, A, B, C, D),  其中会调用子函数 (M, Z) = ComputeCompositesFast(k, B, [C], [D]), 注意Z = k*M
- 校验方:  flag = VerifyProof(A, B, C, D, proof)，其中会调用子函数 (M, Z) = ComputeComposites(B, [C], [D])，注意由于没有k，无法直接通过Z = k*M计算Z，所以会多做加法和乘法

GenerateProof 跟ecdsa_sign比较像，调用ComputeCompositesFast计算M/Z，随机数r还要跟Base/M算点乘得到t2/t3

ComputeCompositesFast支持批量blindToken/elements的计算，主要是调hash、结合点乘计算，得到M/Z，用于协助GenerateProof计算proof

VerifyProof 跟ecdsa_verify比较像，调用ComputeComposites计算M/Z，使用proof/M/Z恢复t2/t3


OPRF Protocol
==========================================================

.. math::

    Server的公私钥对为(skS, pkS), 其中pkS = Base * skS，Base为Group G的生成元

    Client:
        //blind为随机数，P = hash_to_group(input),  blindElement = P * blind
       blind, blindElement = Blind(input) 

    Server:
        // evaluatedElement = blindElement * skS
       evaluatedElement = Evaluate(skS, blindElement)  

    Client:
        // 先求blind的逆blind_inv
        // 再求 N = evaluatedElement * blind_inv = blindElement * skS * blind_inv = P * blind * skS * blind_inv = P * skS
        // 结合 input、N 计算hash，做为output
        output = Finalize(input, blind, evaluatedElement, blindElement) 

VOPRF Protocol
==========================================================

.. math::

    Client:
       //blind, blindElement 计算同OPRF
       blind, blindElement = Blind(input) 

    Server:
       //evaluatedElement 计算同OPRF
       //计算proof = GenerateProof(skS, Base, pkS, blindElement, evaluatedElement)
       (evaluatedElement, proof) = Evaluate(skS, pkS, Base, blindElement)  

    Client:
       // 先调用VerifyProof(Base, pkS, blindElement, evaluatedElement, proof) 进行校验
       // 后续计算同OPRF
       output = Finalize(input, blind, evaluatedElement, blindElement, proof)

POPRF Protocol
==========================================================
    
.. math::

    Client: 
       //基于info派生m，计算T = Base * m，tweakedKey = T + pkS
       //blind, blindElement 计算同OPRF
       (blind, blindElement, tweakedKey) = Blind(input, info)
    
    Server:
       //基于info派生m，计算 t = m + skS，显然 tweakedKey = Base * t
       //计算t的逆t_inv，evaluatedElement = blindElement * t_inv，显然, blindElement = evaluatedElement * t
       //计算proof = GenerateProof(t, Base, tweakedKey, evaluatedElement, blindElement)
       (evaluatedElement, proof) = Evaluate(skS, pkS, Base, blindElement, info)

    Client:
       //先调用VerifyProof(Base, tweakedKey, evaluatedElement, blindElement, proof)
       // 后续计算同OPRF
       output = Finalize(input, blind, evaluatedElement, blindElement, proof, info, tweakedKey)

security
==========================================================

pseudorandomness: 明显是indistinguishable的

non-malleable: 无法锻造evaluation

Oblivious: server无法反推input，client无法反推skS。即，unlinkability。

verifiable: 可使用pkS校验output

pre-image & collision resistant : hash_to_group

timing leaks: constant time operation => GenerateProof/VerifyProof

key rotation: compromised

pre-process:  multiplicative blinding: r*hash_to_group(input)  与   additive blinding: hash_to_group(input) + r*Base，显然后者可以通过预计算加速处理。改Blind/Unblind就行了。
