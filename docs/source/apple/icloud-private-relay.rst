icloud private relay
=========================

https://www.apple.com/privacy/docs/iCloud_Private_Relay_Overview_Dec2021.PDF

https://blog.cloudflare.com/icloud-private-relay

ios15 推出。

不是VPN，使用2层relay。
- device使用relay2的公钥加密website name，relay1无法知道website name。
- device通过relay1代理与relay2的连接，relay2无法知道device IP address。
- relay2代理与website的连接。

选择relay2的address：
- 直接使用device ip所在的country & timezone选择对应的relay2 address。
- Device从relay1获取geohash，基于geohash选择relay2 address。

使用MASQUE中的SECURE PROXYING实现proxying connections。

device校验TLS hanshake中提供的raw public key，并与预设内容进行比较，确认proxy可信。——TLS ESNI。

Relay1发tls client hello，relay2解密website name后连接website，website返回tls handshake。

device使用 oDoH 对relay1隐藏domain name。——ODoH查询结果用于tls esni。

隐私风险：device从relay1获得device自身的public subnew，用于odns查询时提供给最终的dns server。——此时，相当于泄漏client subnet到dns权威。

Safari & HTTP 无需使用oDoH，直接通过proxy连接，而非device。

Device -> relay1 ：采用account attestation认证，再分发anonymous token（RSA blind signature）—— privacy pass。

通过token限制访问。


安全分析
---------

- ESNI可能被屏蔽，或反向识别。
- 往odoh里加client subnet的隐私风险。
- geohash的风险（较小），仅用于select server。
- Relay1 + relay2 合谋风险。

