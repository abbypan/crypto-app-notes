Protocol for Protecting Against Impersonation
###################################################

例如，client 生成随机数x，monitor返回 a = f(x, w)，client 发送challenge c，monitor返回 z = g(x, w, a, c)，client最终校验。

hash
==========================================================

challenge: client与server共享某个S。

    client -> server 一个challenge R, server返回hash(S|R)，client校验该hash；

    server -> client一个challenge P，client返回hash(S|P)，server校验该hash。

完整性：client与server共享某个S。

    client -> server : hash(S|M)，其中M为消息内容。此时该hash相归于消息验证码

password 
==========================================================

加密一个hashed password的数据库

存储一个 hash(hash .. hash(password | salt | server ))

Lamport's Hash­Problems 问题，并非双向认证，client无法鉴别server，因此client存在被中间人欺骗的风险

Strong Password Protocols
==========================================================

先假设client与server共享一个weak key W = hash(password)

    client -> server :  W(g^a mod p)

    server -> client :  W(g^b mod p, C) ，其中C为server到client的challenge

    client -> server :  K(C, D)，其中K ＝ g^(ab) mod p，D为client到server的challenge

    server -> client :  K(D)

SPEKE
==========================================================

Strong Password Protocols ­ 

    let g ＝ W^2 mod p

    client -> server :  K_c = g^a mod p

    server -> client :  K_s = g^b mod p

    双方共享的 K = g^(ab) mod p

Two Factor Authentication
==========================================================

例如，password + pin

KDC
==========================================================

Key Distribution Centers

例如A想与B通信，A/B均与KDC相连。

KDC将密钥R加密传给A。

KDC使用B的key加密R获得T，将T传给A。

A使用R加密消息M，获得C，将（C，T）传给B。

B使用自身的key解密T获得R，使用R解密C获得M。

问题在于KDC全控制。

CA
==========================================================

使用相同CA颁发的证书相互认证通信。

certificate必须与name关联。

