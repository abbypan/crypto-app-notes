ZKP
######

`MOOC: Zero Knowledge Proofs <https://zk-learning.org/>`_

Lecture 2 Dan Boneh: Introduction to Modern SNARKs
==========================================================

zk-SNARK: Zero-knowledge a Succinct ARgument of Knowledge

setup for circuit C: 
- trusted setup per circuit: S(C; r) -> (pp, vp), secret r
- trusted but universal setup: Sinit(λ; r) -> gp, secret r 仅初始化一次; Sindex(gp, C) -> (pp, vp),  per circuit
- transparent setup: S(C) -> (pp, vp)

SNARK (S, P, V)
- S(C) -> (pp, vp), public parameters
- P(pp, x, w) -> π, proof
- V(vp, x, π) -> accept/reject

functional commitment
- setup(1^λ) -> gp
- commit(gp, f, r) -> comf, r为random, f为function
    - polinomial
    - multilinear
    - vector
    - inner product arguments (IPA)
- eval(P, V): P(gp, f, x, y, r) -> π,  V(gp, comf, x, y, π) -> accept/reject

interactive oracle proof (IOP)

Lecture 5 Dan Boneh: The Plonk SNARK
==========================================================

Poly-IOP: Zero Test, Sum Check, Prod Check, Permutation Check

PLONK: a poly-IOP for a general circuit 𝐶(𝑥, 𝑤)，构造input, gate, wiring, output的check，使用lagrange & FFT

Lecture 6 Yupeng Zhang: Polynomial Commitments based on Pairing and Discrete Logarithm
==========================================================

Univariate KZG, Multivariate KZG, ...

Bulletproofs


Scalable Zero Knowledge via Cycles of Elliptic Curves
==========================================================

`Scalable Zero Knowledge via Cycles of Elliptic Curves <https://eprint.iacr.org/2014/595.pdf>`_

