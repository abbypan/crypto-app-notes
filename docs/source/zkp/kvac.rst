KVAC
=======

`The Signal Private Group System and Anonymous Credentials Supporting Efficient Verifiable Encryption <https://www.semanticscholar.org/paper/The-Signal-Private-Group-System-and-Anonymous-Chase-Perrin/dd8ec2ccb7c91c6a6352d341032d1d7746283c6f>`_

注意这里zero knowledge proof的assume condition：

- uid的proof由server校验，因此用sk做verify，无需public 。

- user profile的proof由group member使用shared key校验。group management的access control。

- 利用elgamal encryption的同态特性来做blinded attr/public verifiable。

其他思路与group signature相似。
