BitID
########

`Bitcoin address authentication protocol <https://github.com/bitid/bitid/blob/master/BIP_draft.md>`_

场景示例
==========================================================

站点提供二维码形式的bitid URI，例如 bitid:www.site.com/callback?x=NONCE

Bitcoin的app扫描之，并选择自身的私钥，签名该URI；并将该签名以及自身公钥POST到callback url。

callback url 收到信息后比对，登陆。

nonce有超时机制防重放。

比较
==========================================================

`Zooko's triangle <https://en.wikipedia.org/wiki/Zooko's_triangle>`_: 安全，可读，去中心化

BitID 安全ok，去中心化ok，可读性相对一般。

与FIDO UAF认证的思路比较像，差别是，FIDO UAF更偏向于硬件支持（底层设备支持），BitID基于Bitcoin构建（密钥管理外包了）。

软硬件层面的完备性不好说，但是可读性还是可以优化的。
