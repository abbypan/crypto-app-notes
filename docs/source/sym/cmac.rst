AES-CMAC
==========

key & padding
--------------

基于原始的16字节K会派生出两个16字节的sub key: K1, K2。

当原始消息已经block size对齐时，使用K1参与最后一个block的计算；

否则，padding(x) 会把原始消息M补齐到block size对齐，使用K2参与最后一个block的计算。

padding(x)：在M后面拼接一个1，后面再带i个0

aes
-----

默认是使用aes-128-cbc

maclen为16字节


参考资料
---------

- `RFC4493: The AES-CMAC Algorithm <https://www.rfc-editor.org/rfc/rfc4493.html>`_
