PKI
########


结构
==========================================================

Hierarchical CA，单Root，例如DNSSEC

Peer-to-Peer CA，多个Root CA之间互签，相当于每次都是1v1互签

Bridge CA，多个Root CA各自与Bridge CA互签，Bridge CA作为中介，连接起不同联盟的CA

信任
==========================================================

CA Trust List，例如浏览器CA List

本地 Root CA + Bridge，例如美国federal bridge

安全
==========================================================

单CA + 无Bridge，应用范围有限，适用于自建CA自行应用，无交互

P2P CA，有限度的互相信任，可随时主动撤销信任；适用于群组内强互信的场景

CA Trust List，比较宽松的信任，一个CA可恶意签发由其他CA签发的域名；适用于上级层数较少、底层叶子极多、CA不互通的场景

Bridge，更加宽松的信任，Bridge传递信任，然而加入的CA越多越不可控；且验签链路较长；适用于相同业务的不同CA信任联盟

资料
==========================================================

- `PKI and Certificate Security <https://coinsrs.no/wp-content/uploads/2018/05/finse2018-presentation-gruschka-nils.pdf>`_
- `RFC4158 Internet X.509 Public Key Infrastructure: Certification Path Building <https://tools.ietf.org/html/rfc4158>`_
- `RFC5217 Memorandum for Multi-Domain Public Key Infrastructure Interoperability <https://tools.ietf.org/html/rfc5217>`_
- `Introduction to Public Key Infrastructure <https://ncvhs.hhs.gov/wp-content/uploads/2014/05/050113p3.pdf>`_
- `Understanding Certification Path Construction <http://www.oasis-pki.org/pdfs/Understanding_Path_construction-DS2.pdf>`_
- `The Federal Bridge: A Foundation of Trust <https://www.entrust.com/wp-content/uploads/2013/05/federal_bridge.pdf>`_
- `PKI Trust Models <https://pdfs.semanticscholar.org/6ec1/d42d93b734548555110f2e0afa321533b8ba.pdf>`_
- `The Federal Bridge Certification Authority <http://www.oasis-pki.org/members_only/meetings/nov2002/d1-p5-dod-usa.pdf>`_
- `Public Key Infrastructures <http://www.hit.bme.hu/~buttyan/courses/BMEVIHIM219/2014/slides-pki-tech.pdf>`_
- `RFC5272 Certificate Management over CMS (CMC) <https://tools.ietf.org/html/rfc5272>`_
- `RFC7030 Enrollment over Secure Transport <https://tools.ietf.org/html/rfc7030>`_
- `PKI Trust Models: Whom do you trust? <https://www.sans.org/reading-room/whitepapers/vpns/pki-trust-models-trust-36112>`_
- `Cross-Certification and PKI Policy Networking <https://www.netrust.net/docs/whitepapers/cross_certification.pdf>`_
- `PKI-in-nutshell <https://swedbank.ee/download/gateway/PKI-in-nutshell.pdf>`_
- `Usability and Key Management <https://csrc.nist.gov/CSRC/media/Presentations/Usability-and-Key-Management/images-media/Usability_and_Key_Mgmt.pdf>`_
