PAKE Combiners
=================

- `PAKE Combiners and Efficient Post-Quantum Instantiations <https://eprint.iacr.org/2024/1621.pdf>`_
 
Figure 2, password-hiding PAKE: CPace, EKE, SPAKE2

Figure 4, post-quantum statistically PSK-equality-hiding PAKE: CAKE, EKE-PRF

ParComb combiner for PAKEs: pw 并行用于2个PAKE协商，最后合并派生session key

SeqComb combine for PAKEs:  pw 用于第1个PAKE协商，使用首个PAKE协商的Key派生Z；Z用于第2个PAKE的协商；最后合并派生session key



