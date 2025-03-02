Apple HAP
###############

`HomeKit Accessory Protocol Specification <https://developers.apple.com/homekit/faq/>`_

device/controller 指 ios device。

accessory 指物联网设备。

core concepts
==========================================================

security
----------------------------------------------------

e2ee, mutually authenticated,  forward secrecy, 每次session重新生成unique key。

pairing setup 主要用于ios device 与 accessory 相互安全交换 long-term public key。用户在ios device侧输入8-digit setup code。setup code 由 accessory 通过屏幕/标签 提供，用于SRP协议校验。

pairing verify 主要用于ios device 与 accessory 相互基于 long-term public key 进行安全通信，建立ephemeral shared secret，派生session key。

attributes
==========================================================

service
----------------------------------------------------

service 表示 服务类型。

primary service: accessory 应指定 primary service，即默认service。

hidden service: 对用户不可见。

linked service: A -> B，标识A service 依赖 B service，单层依赖。

characteristics
----------------------------------------------------

characteristics 表示功能特性

perms 权限

ev 通知(notification)

additional authorization data : 由accessory 的ios app 提供给ios，安全存储。定期更新。当ios device发控制指令时，可以附加additional authorization data，由accessory进行访问控制。

roles
==========================================================

hap client: controller，即ios device。

hap accessory server:  accessory，即物联网设备，hap accessory server 可以列出其下属的多个物联网设备hap accessory objects。
- primary hap accessory object: instance ID 编号为1。

hap accessory objects: accessory 物联网设备，单设备。

bridge：hap accessory server，用于zigbee/z-wave等异构网络 与 hap 网络桥接。
- bridge必须确保hap accessory object的instance ID，在配对有效期内不变。
- 涉及physical access to the home的accessory，不能被bridged。
- 自身支持IP的accessory，不能被bridged。
- 只提供传感数据的BLE accessory，或者Zigbee等异构网络的accessory，可能被bridged。


accessory attribute database
==========================================================

accessory server 提供的accessory objects, service objects, characteristic objects 列表。

在server/client的配对有效期内不变。

注意新firmware升级后，如果移除某些service/characteristic，则其instance ID不能被新的其他service/characteristic替换。

requirements
==========================================================

accessory 与 ios device 在同一wifi network里配对。

主要支持hap for IP & hap for BLE。

accessory setup
==========================================================

setup code 应安全随机生成，device unique，禁止通过因子派生。

如果accessory在工厂设置setup code，则必须存储srp verifier，而非setup code本身。

pairing
==========================================================

推荐安全存储于secure element。

device ID：accessory在每次factory reset时随机生成identifier。


pair setup
----------------------------------------------------

采用SRP， ios device 输入 setup code，accessory 进行verify，协商shared secret (S)，基于S派生session key (K)。

双方在K的保护下，安全交换对方的long-term public key (ed25519)。

以ios device为例，

.. note::

    iOSDeviceX = hkdf-sha-512(input key = S, salt = "pairing-setup-controller-sign-salt", info = "pair-setup-controller-sign-info", L = 32)
    iOSDeviceInfo = iOSDeviceX || iOSDevicePairingID (配对标识) || iOSDeviceLTPK (长期公钥)
    iOSDeviceSignature = sign(iOSDeviceLTSK, iOSDeviceInfo)  使用长期私钥签名
    基于 { iOSDevicePairingID, iOSDeviceLTPK, iOSDeviceSignature } 构造TLV序列 Sub-TLV
    encryptedData, authTag = ChaCha20-Poly1305( K, Nonce = "PS-Msg05", AAD= <none>, Msg = <Sub-TLV>)

accessory收到encryptedData, authTag后，解密、校验、安全存储iOSDevicePairingID, iOSDeviceLTPK

pair verify
----------------------------------------------------

采用Station-to-Staion(STS)协议，进行双向认证。

.. note::

    ios device -> accessory :  ios device's x25519 temp public key

    accessory -> ios device :  
        accessoryInfo = { accessory's x25519 temp public key, accessoryPairingID, ios device's x25519 temp public key }
        accessorySignature = sign(accessLTSK, accessoryInfo)
        基于 { accessoryPairingID, accessorySignature }构造TLV序列 Sub-TLV
        基于双方x25519 temp public key构造shared secret (S)。
        基于S构造session key，即 K = hkdf-sha-512(input key = S, salt = "pair-verify-encrypt-salt", info = "pair-verify-encrypt-info", L = 32)
        encryptedData, authTag = ChaCha20-Poly1305( K, Nonce = "PV-Msg02", AAD= <none>, Msg = <Sub-TLV>)
        向ios device发送 { accessory's x25519 temp public key, encryptedData, authTag }

    ios device -> accessory : 
        iosDevice同样派生K解密，并根据accessoryPairingID定位accessoryLTPK, 验签。
        iosDeviceInfo = { iosDevice's x25519 temp public key, iosDevicePairingID, accessory's x25519 temp public key }
        iosDeviceSignature = sign(accessLTSK, iosDeviceInfo)
        基于 { iosDevicePairingID, iosDeviceSignature }构造TLV序列 Sub-TLV
        encryptedData, authTag = ChaCha20-Poly1305( K, Nonce = "PV-Msg03", AAD= <none>, Msg = <Sub-TLV>)
        向accessory发送 { encryptedData, authTag }

    该session的后续通信，使用 前32-bit为0 的 64-bit nonce 用于 ChaCha20-Poly1305：
        AccessoryToControllerKey = hkdf-sha-512(input key = S, salt = "control-salt", info = "control-read-encryption-key", L = 32)
        ControllerToAccessoryKey = hkdf-sha-512(input key = S, salt = "control-salt", info = "control-write-encryption-key", L = 32)


add pairing
----------------------------------------------------

ios device -> accessory : add pairing request

remove pairing
----------------------------------------------------

ios device -> accessory : remove pairing request

list pairing
----------------------------------------------------

ios device -> accessory : list pairing request

accessory -> ios device : `[ { controller_i pairing identifier, controller_i's LTPK, controller's permissions}, ... ]`

pairing over BLE
----------------------------------------------------

HAP characteristic 在 BLE 的max payload size <= 512 bytes


hap for IP accessories
==========================================================

hap accessory server
- 基于mdns广播host name，例如lights.local。
- name collision => RFC6762
- 广播dns service，包含TXT RR。

http/1.1 + json

accessory instance id => aid, integer

service instance id => iid, integer

characteristic instance id => iid, integer

servie type => uuid

characteristic type => uuid

discovery
----------------------------------------------------

service type: _hap._tcp 

_hap._tcp 的TXT RR 中，包含
- id = accsory device ID，即accessory's pairing identifier。
- model name
- current state number
- accessory category identifier
- ...

security for IP accessories
----------------------------------------------------

可以基于hap connection安全协商其他协议的安全参数，再应用其他协议安全通信，例如SRTP等。

session下安全通信，传输数据为 { n = length of ciphertext (2 bytes), ciphertext (n bytes), authTag (16 bytes) }

homekit data stream (HDS)
----------------------------------------------------

基于当前session的shared secret (S), 派生数据传输的encryption key：

.. note::

        AccessoryToControllerBulkTransferEncryptionKey = hkdf-sha-512(input key = S, salt = <controllerKeySalt><accessoryKeySalt>, info = "HDS-read-encryption-key", L = 32)
        ControllerToAccessoryBulkTransferEncryptionKey = hkdf-sha-512(input key = S, salt = <controllerKeySalt><accessoryKeySalt>, info = "HDS-write-encryption-key", L = 32)

CHACHA20-POLY1035

hap for BLE
==========================================================

service & characteristic 使用standard GATT

BLE version >= 4.2

pair-resume procedure
----------------------------------------------------

快速重连，避免EC计算。

至少支持 8 sessions restore。

基于当前session的shared secret (S), 派生session ID = hkdf-sha-512(input key = S, salt = "pair-verify-resumesessionid-salt", info = "pair-verify-resumesessionid-info", L = 8)

controller -> accessory: 

.. note::

    随机生成controller's x25519 temp public key
    RequestKey = hkdf-sha-512(input key = S, salt =<Controller's x25519 temp public key><session ID>, info = "pair-resume-request-info", L = 32)
    encryptedData, authTag = ChaCha20-Poly1305(RequestKey, Nonce="PR-Msg01", AAD=<none>, Msg=<0 byte RequestData>)
    向accessory发送 { controller's x25519 temp public key, session ID, authTag }

accessory -> controller: 

.. note::

    accessory根据session ID定位shared secret。
    如果shared secret已过期，则触发pair verify(直接使用当前收到的controller's x25519 temp public key，节省RTT)，结束resume procedure。
    如果shared secret存在，则派生RequestKey，校验authTag。
    随机生成new session ID, 派生ResponseKey = hkdf-sha-512(input key = S, salt =<Controller's x25519 temp public key><new session ID>, info = "pair-resume-response-info", L = 32)
    encryptedData, authTag = ChaCha20-Poly1305(ResponseKey, Nonce="PR-Msg02", AAD=<none>, Msg=<0 byte RequestData>)
    向controller发送 { new session ID, authTag }

双方更新shared secret:

.. note::

    new shared secret = hkdf-sha-512(input key = S, salt =<Controller's x25519 temp public key><new session ID>, info = "pair-resume-shared-secret-info", L = 32)

BLE
----------------------------------------------------

accessory 使用 static random BLE address

accessory应在3个advertising channel (37, 38, 39) 广播。

accessory  ATT_MTU> 100 bytes, MTU>= 150 bytes

hap characteristic <= 512 bytes

accessory 在相关状态变化后，更新16-bit global state number (GSN) ，broadcast通知homekit controller。

accessory 在 factory reset / firmware upgrade 后，重置GSN。

accessory advertising identifier : 6-byte的device ID, 即 accessory's unique pairing identifier。

Msg = { GSN (2 byte), characteristic iid (2 byte), value (8 byte) } 

accessory派生 Broadcast-Encryption-Key = hkdf-sha-512(input key = S, salt = <Controller's LTPK>, info = "Broadcast-Encryption-Key", L = 32)

encrypted-advertisement-payload, authTag (4 byte) = ChaCha20-Poly1305(Broadcast-Encryption-Key, Nonce = GSN, AAD = 48-bit accessory advertising identifier, Msg )

GSN有隐私追踪风险。


