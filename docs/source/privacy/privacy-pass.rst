Privacy Pass
###############

doc
==========================================================

`Privacy Pass Protocol Specification <https://datatracker.ietf.org/wg/privacypass/documents/>`_

参考 RFC9576/9577/9578，主要是9578。

最初是用在TOR。

client -> server 的匿名授权，生成token。

server 无法基于 client 的 re-authorization ， link 跟踪到初始的授权。

token
==========================================================

cookie 的问题就是token跟踪，跨域跟踪。

privacy pass protocol:
- unlinkability: client一次性获得多个single-domain/cross-domain的授权token，不用重复认证，且确保匿名性。
- unforgeability: client无法伪造token，或者增加token个数。

细节参考 voprf

phase:

.. raw::
   
    1) issuer server setup:
    skS, pkS

    2) client setup:
    pkS, m

    3) issuance:
    client: m (input) -> req (blindToken)
    issuer server: resp (evaluation)
    client:
            redemption Token = { input.data, issued: issuedTokens }
    与voprf的issuedTokens过程一致

    4) redemption:
    client:  token, info -> req
        tag = Finalize(token.data, token.issued, info) //info 加 timestamp，生成hash output
        req = redemption request = { data, tag, info }

    issuer server:
        检查是否已遇到过该req.data，避免double spend;
        resp = VerifyFinalize(pkS, skS, req.data, req.info, req.tag) //相当于让server自己做一下签名校验
        如果resp.success，登记req.data


protocol
==========================================================

client 向 origin server 发起访问请求。

origin server 提供 token challenge。

client 与 issuer server 的authentication 参考rats(RFC9334)的attestation。

client 向 issuer server 请求签发 token。

origin server 校验 token。


browser extension usage
==========================================================

`Privacy Pass: A browser extension for anonymous authentication <https://medium.com/@alxdavids/privacy-pass-6f0acf075288>`_

`Privacy Pass - “The Math” <https://blog.cloudflare.com/privacy-pass-the-math/>`_

`Challenge Bypass Extension <https://github.com/privacypass/challenge-bypass-extension>`_

主要针对匿名访问时用户重复输入验证码的问题

利用椭圆曲线交互认证，用户一次获得多个token（默认一次认证成功自动生成30个）

那么下回用户向edge服务器再次请求在cloudflare托管的其他站点内容时，就不用再次输入验证码，cloudflare直接验token即可（该token之前没用过）

可以算做server端防ddos策略影响用户浏览体验的一种折中方案，privacy另议。

Signing phase
----------------------------------------------------------

.. raw::

    C samples a random ‘blind’ r ← ZZ_q  #模为q的整数环
    C computes T = H_1(t) and then blinds it by computing rT  
    C sends M = rT to S
    S computes Z = xM and returns Z to C
    C computes (1/r)*Z = xT = N and stores the pair (t,N) for some point in the future #C无法知道S的私钥x

Redemption phase
----------------------------------------------------------

.. raw::

    C calculates request binding data req and chooses an unspent token (t,N)
    C calculates a shared key sk = H_2(t,N) and sends (t, MAC_sk(req)) to S  #sk即共同密钥
    S recalculates req' based on the request data that it witnesses
    S checks that t has not been spent already and calculates T = H_1(t), N = xT, and sk = H_2(t,N)  #确定t还没用过，用私钥x计算出sk
    Finally S checks that MAC_sk(req') =?= MAC_sk(req), and stores t to check against future redemptions

