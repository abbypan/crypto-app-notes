pairing
###########
    
- `pairing curve: PBC Library <https://crypto.stanford.edu/pbc/>`_
- `Pairings for beginners <https://static1.squarespace.com/static/5fdbb09f31d71c1227082339/t/5ff394720493bd28278889c6/1609798774687/PairingsForBeginners.pdf>`_

BLS12-381
==========================================================

- `Pairing over BLS12-381, Part 1: Fields <https://www.nccgroup.com/us/research-blog/pairing-over-bls12-381-part-1-fields/>`_
- `Pairing over BLS12-381, Part 2: Curves <https://www.nccgroup.com/us/research-blog/pairing-over-bls12-381-part-2-curves/>`_
  
amicable pair
==========================================================

`Amicable pairs and aliquot cycles for elliptic curves <https://arxiv.org/pdf/0912.1831.pdf>`_

amicable pair: (p, q)

.. math::

     ˜Ep(Fp) = q and # ˜Eq(Fq) = p and p<q

基于amicable pair进一步扩展，定义size l 的循环为aliquot cycle

l=1的prime即为anomalous prime，ECDLP降至linear time

CM curves: elliptic curves having complex multiplication

    if E/Q has CM with j(E) 6 = 0, and if q = # ˜Ep(Fp) is prime, then there are only two possible values for # ˜Eq(Fq), namely p and 2q + 2 − p


