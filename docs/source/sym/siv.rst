AES-SIV
##############

RFC5297

aes-siv:  AES-CMAC (S2V) + AES-CTR

aes-siv-key = aes-cmac-key (k1) || aes-ctr-key (k2)

aes-siv-key 256 bits起步，k1 || k2 前后各半

authenticated encryption

key wrapping，替代rfc3394

抵御nonce的reuse、misuse

key derivation，string to vector (s2v)

message authentication

性能相比gcm差

S2V：将原文拆分block，再链式调用aes-cmac，异或处理，最终获得128 bits的输出V。

SIV：将原文与additional data，结合k1，算出S2V输出V。将V作为CTR的IV，结合K2和原文，计算密文C。返回 V || C。
