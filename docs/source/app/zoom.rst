zoom security
##################


概要
======================================

zoom 之前被喷 aes ecb，以及伪的e2e。

zoom 还支持 webrtc (浏览器接入)， pstn （电话接入）。支持sip, h.323。

支持1k人的会议，听+说。

zoom server生成256-bit的 per-meeting key；基于per-meeting key，结合stream ID，派生 per-stream key

per-stream key用于aes-gcm加密音频、视频的udp包。

每个client有独立的uniquely-identified stream。zoom的MMR SERVER负责转发这些加密包。

当PSTN or SIP接入，MMR把per-meeting key分发给zoom的代理server，由代理server做为特殊的zoom client，解密视频流。

当前是zoom生成meeting key，再派生per-client key/per-stream key。

整改
======================================

client
----------------------------------------------------

每个zoom client i 登记 device id -> long-live public key ( IVK_i )

long-live private key 为 ISK_i

建立long-live public key的transparency tree，类似证书透明化的操作。

client i 加入某个meeting时，生成临时的key pair (pk_i, sk_i)，并生成一个

    binding_i = (meetingID || meetingUUID || i || device ID || IVK_i || pk_i )

使用自身的long-live private key (ISK_i) 签发 

    Sig_i = sign( ISK_i, "zoom00epubkeys\0" || binding_i )

所有的参与者都可以校验Sig_i


leader
----------------------------------------------------

leader加入meeting，

先从zoom获取meetingUUID，随机生成256bit的seed mk，再派生per-meeting key:

    MK = HKDF(mk, "zoom00skey\0" || meetingID || meetingUUID )

从MMR获取参与本会议的client列表，对每个client_i，leader：

校验 Sig_i

生成 

    Meta <- ("zoom00sdkey\0" || meetingID || meetingUUID || l || i ) , l 为 leader标记

基于leader的 skg_l 与 pk_i 计算 DH share key，再加入Meta，派生临时密钥，加密mk，获得密文C_i。

公布(i, C_i)。

client join
----------------------------------------------------

client i 从zoom获取leader的信息，并校验Sig_l

获取(i, C_i)，解密得到mk，然后派生出MK。

leader participant list (LPL)
----------------------------------------------------

v为计数器，每次LPL有变化就+1

t为时间戳

mkSeqNum为mk的计数器，每次生成新的mk就+1；每个加密的udp包含了明文的mkSeqNum，所以可以知道该udp包是用哪个mk派生出的MK。

leader以自身 ISK_l 签发 

    Sig(ISK_l, "Zoom00LPL\0" || binding_l || HSA256(LPL_v) || v || t || mkSeqNum )

meeting leader security code
----------------------------------------------------

    meeting leader security code = Wordify(HASH128("Zoom00MSecCode\0" || IVK_l))

leader显示该code，client可以计算并校验

本地long-live private key保护
----------------------------------------------------

可以用key wrapping key保护 ISK_i

client_i 随机生成 KWK_s，并在server登记

基于 KWK_s 派生 

    KWK = HKDF(KWK_s, "zoom00LKS\0")

使用KWK加密 ISK_i，获得密文C，client_i 本地存储密文C。


分析
======================================

这个反正也不是完全的e2e，存在代理client解密。

此外public key的分发由zoom负责，透明化的public key审计凑合看看。

KWK保护ISK_i的场景，就看KWK_s有多长，可以暴力破解。


参考资料
=============

- `End-to-End Encryption for Zoom Meetings <https://github.com/zoom/zoom-e2e-whitepaper>`_
- `does zoom use e2e encryption <https://blog.cryptographyengineering.com/2020/04/03/does-zoom-use-end-to-end-encryption/>`_
- `If Zoom Is Wrong, So Is Apple <https://sneak.berlin/20200604/if-zoom-is-wrong-so-is-apple/>`_
- `Zoom says it won’t encrypt free calls so it can work more with law enforcement <https://news.ycombinator.com/item?id=23399924>`_

