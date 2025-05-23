MLS: Message Layer Security
################################

overview
============


设计思路跟apple/zoom比较类似，目标是使得key server（看似）无法获取/监听实际group消息，（尽量）避免ghost node的场景。

核心是merkle tree的TreeKEM。

每个leaf node能够知道其路径上的所有上级节点的公私钥对。

当某个leaf要在限定范围的group做安全通信时，leaf用该限定范围内的group的子树的根节点priv key派生出对应leaf node的sender key，再加密。

此时，能够保证指定范围内的安全通信。

key
======================================

client identity: long term key。

通过long term key发布signature key、initialization key(InitKey，use only once)。发布的形式可以是HPKE的key package。

key package里还可以设定一个该client发布的数字信封公钥，方便接收方与其进行e2e通信。

security
======================================

post-compromise 的效果要看member更新leaf key的周期。group secret的更新，无法抵御leaf key泄漏的场景。

forward-secrecy 的效果主要看delete old priv key & delet message encrypt key

参考资料
======================================

- `ietf mls protocol <https://datatracker.ietf.org/doc/draft-ietf-mls-protocol/>`_
- `draft-ietf-mls-protocol <https://www.ietf.org/proceedings/105/slides/slides-105-mls-sessa-mls-protocol-00-01>`_
- `Better Encrypted Group Chat <https://blog.trailofbits.com/2019/08/06/better-encrypted-group-chat/>`_
- `Multicast Key Agreement, Revisited <https://www.youtube.com/watch?v=ySdNHvlDwFI>`_
- `TCC 2022 session on Protocols Key Agreement and Commitments <https://www.youtube.com/watch?v=qXnELNvSDlM>`_
- `mls <https://i.blackhat.com/USA-19/Wednesday/us-19-Robert-Messaging-Layer-Security-Towards-A-New-Era-Of-Secure-Group-Messaging.pdf>`_
