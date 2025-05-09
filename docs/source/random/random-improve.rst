Randomness Improvements
###########################

`RFC 8937: Randomness Improvements for Security Protocols <https://www.rfc-editor.org/rfc/rfc8937.html>`

cryptographically secure" pseudorandom number generators (CSPRNGs) 的问题在于，熵值的强度。

naxos把随机数x用H(x, sk)处理一下作为random用，其中sk为sender的私钥。

借鉴naxos的思路，把H(x, sk)替换为签名操作Sig(sk, tag1)，tag1/tag2固定，G(L)为原始random生成器，`G'(n) = Expand(Extract(H(Sig(sk, tag1)), G(L)), tag2, n)`

Sig(sk, tag1)的值可缓存，hsm接口兼容性好一点？！


issue
==========================================================

seed
--------

`When private keys are public: results from the 2008 Debian OpenSSL vulnerability <https://pdfs.semanticscholar.org/fcf9/fe0946c20e936b507c023bbf89160cc995b9.pdf>`_

algorithm
-------------

`Dual EC: A Standardized Back Door <https://projectbullrun.org/dual-ec/documents/dual-ec-20150731.pdf)>`_
