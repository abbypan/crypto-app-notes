asn.1
#########

结构
==========================================================

  tag - length - value

其中，value中可以再嵌套 tag - length - value

tag 
==========================================================

`Encoded Tag Bytes <https://docs.microsoft.com/zh-cn/windows/desktop/SecCertEnroll/about-encoded-tag-bytes>`_

`ASN.1 Listing of Universal Tags <https://www.obj-sys.com/asn1tutorial/node124.html>`_

第8、7位标识tag class：{ universal 00, application 01, context-specific 10, private 11 }

第6位标识是否constructed: { 0 primitive, 1 constructed }

后5位标识tag number

implicit vs explicit
----------------------------------------------------

`ASN.1 tagging principles <http://powerasn.ncottin.net/download/ASN1_Tagging.pdf>`_

`IV. ASN.1 <http://www.ee.oulu.fi/research/tklab/courses/521265A/lectures/ch4_ASN1.pdf>`_

`X.690 <https://www.itu.int/ITU-T/studygroups/com17/languages/X.690-0207.pdf>`_

默认explicit

.. note::

    Type1 ::= VisibleString 
    Type2 ::= [APPLICATION 3] IMPLICIT Type1 
    Type3 ::= [2] Type2 
    Type4 ::= [APPLICATION 7] IMPLICIT Type3 
    Type5 ::= [2] IMPLICIT Type2

value: Jones

.. note::
   
    Type1: 
    0x1A=0b00011010 0x05 0x4A6F6E657

    Type2: 
    Application 01, tag number 替换为3
    0b01000011 = 0x43  0x05 0x4A6F6E657

    Type3: 默认explicit, explicit为context-specific 10 且structured 1, tag number替换为2
    0b10100010 = 0xa2 0x07 0x43 0x05 0x4A6F6E657

    Type4: Application 01, tag number替换为07，structured 1不变
    0b01100111 = 0x67 0x07 0x43 0x05 0x4A6F6E657

    Type5: implicit为context-specific 10，tag number替换为2, 保持原来Type2的primitive不变
    0b10000010 = 0x82 0x05 0x4A6F6E657
    

length
==========================================================

`Encoded Length and Value Bytes <https://docs.microsoft.com/zh-cn/windows/desktop/SecCertEnroll/about-encoded-length-and-value-bytes>`_

第8位为0，标识short form length。后7位标识length值。

第8位为1，标识long form length。后7位标识length取值的字节数，随后的字节数取值即为实际length值。

第8位为1，如果后7位的字节数为0，则标识长度不定，遇0x00 0x00停止。

value
==========================================================

oid 
----------------------------------------------------

`OBJECT IDENTIFIER <https://docs.microsoft.com/zh-cn/windows/desktop/SecCertEnroll/about-object-identifier>`_

OBJECT IDENTIFIER (oid) 的 value 进行了压缩

前两位合并成1个字节： x_1 * 40 + x_2 

后续位数，如果<128，则映射为单字节；如果>=128，则第8bit 置1，后7bits做为128的倍数处理，<128的余数单独编列为1字节。

bit string
----------------------------------------------------

`BIT STRING <https://docs.microsoft.com/zh-cn/windows/desktop/SecCertEnroll/about-bit-string>`_

bit string 的 value 部分的首个字节，标识了将该bit string长度填充为8的倍数所需的bit数


示例
==========================================================

`Reading encode asn file manually <https://stackoverflow.com/questions/34767067/reading-encode-asn-file-manually>`_

30 82 02 10 04 01 56 …
----------------------------------------------------------

.. note::

    tag: 0x30 = 0b00110000 

        class = 00 universal

        constructed = 1 yes

        tag number = 0b10000 = 16 = SEQUENCE and SEQUENCE OF

    length: 0x82 = 0b10000010, 

        long form length : 1

        length's bytes number = 2 

        length = 0x0210 = 528

    value: 04 01 56 ...

30 80 04 03 56 78 90 00 00
----------------------------------------------------

tag: 同前

length: 0x80 = 0b10000000, 

    long form length : 1

    标识后面字节数不定

value:

    04 03 56 78 90  :  tag 04, length 03, value 56 78 90
    
    00 00 : tag 00, length 00

df 82 02 05 12 34 56 78 90
----------------------------------------------------

tag: df 82 02

    0xdf = 1101 1111 : 11 class private, 0 primitive, 11111 全1标识long tag encoding
    
    0x82 = 1000 0010 : 1 后面的字节还是tag number取值， 0b0000010 = 2 

    0x02 = 0000 0010 : 0 是tag number取值的最后一个字节，0b0000010 = 2

    tag number = 0b00000100000010 = 258

length: 05

value: 12 34 56 78 90

oid 1.3.6.1.4.1.311.21.20 
----------------------------------------------------

.. note::

    06 09                                ; OBJECT_ID (9 Bytes)
    |  2b 06 01 04 01 82 37 15 14

    0x06 : tag object

    0x09 : length

    0x2b : 1*40 + 3 = 0x2b

    311 = 128*2 + 55 = 0b10000010 + 0b00110111 = 0x82 + 0x37


bit string 011011100101110111
----------------------------------------------------

    0110 1110 0101 1101 11xx xxxx

    应填充6个bit: 0110 1110 0101 1101 1100 0000

    tag: 0x03

    length: 0x04

    value: 0x06 填充6个bit，后面3个字节以填充后的padding直接转换 0x6e 0x5d 0xc0

    => 03 04 06 6e 5d c0  (short form length)

    => 03 81 04 06 6e 5d c0 (long form of length octets)

    => 23 09   03 03 00 6e 5d   03 02 06 c0 (constructed)

DER vs BER
==========================================================

DER是BER的子集, 对每个ASN.1值只有唯一一种编码方法

DER对短型长度、长型长度、隐式标签简单定长、显式标签结构化定长等场景做了限制。


参考资料
==========================================================

- `ASN.1: Introduction <https://is.muni.cz/el/1433/podzim2015/PV181/um/asn1/ASN1_intro.pdf>`_
- `ASN.1/BER/DER <https://files-cdn.cnblogs.com/files/sutong/ASN1BERDER%E7%BC%96%E7%A0%81%E5%AD%90%E9%9B%86%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97.pdf>`_
- `Parsing BER and DER encoded ASN.1 Objects <https://blog.engelke.com/2014/10/17/parsing-ber-and-der-encoded-asn-1-objects/>`_
- `Introduction to ASN.1 Syntax and Encoding <https://docs.microsoft.com/zh-cn/windows/desktop/SecCertEnroll/about-introduction-to-asn-1-syntax-and-encoding>`_
- `ASN.1 Complete <https://www.oss.com/asn1/resources/books-whitepapers-pubs/larmouth-asn1-book.pdf>`_
- `ASN.1 JavaScript decoder <http://www.lapo.it/asn1js/#MIIF4DCCBMigAwIBAgIQDACTENIG2-M3VTWAEY3chzANBgkqhkiG9w0BAQsFADB1MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMRkwFwYDVQQLExB3d3cuZGlnaWNlcnQuY29tMTQwMgYDVQQDEytEaWdpQ2VydCBTSEEyIEV4dGVuZGVkIFZhbGlkYXRpb24gU2VydmVyIENBMB4XDTE0MDQwODAwMDAwMFoXDTE2MDQxMjEyMDAwMFowgfAxHTAbBgNVBA8MFFByaXZhdGUgT3JnYW5pemF0aW9uMRMwEQYLKwYBBAGCNzwCAQMTAlVTMRkwFwYLKwYBBAGCNzwCAQITCERlbGF3YXJlMRAwDgYDVQQFEwc1MTU3NTUwMRcwFQYDVQQJEw41NDggNHRoIFN0cmVldDEOMAwGA1UEERMFOTQxMDcxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1TYW4gRnJhbmNpc2NvMRUwEwYDVQQKEwxHaXRIdWIsIEluYy4xEzARBgNVBAMTCmdpdGh1Yi5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCx1Nw8r_3zTu3BZ63myyLot-KrKPL33GJwCNEMr9YWaiGwNksXDTZjBK6_6iBRlWVm8r-5TaQMKev1FbHoNbNwEJTVG1m0Jg_Wg1dZneF8Cd3gE8pNb0Obzc-HOhWnhd1mg-2TDP4rbTgceYiQz61YGC1R0cKj8keMbzgJubjvTJMLy4OUh-rgo7XZe5trD0P5yu6ADSindvEl9ME1PPZ0rd5qM4J73P1LdqfC7vJqv6kkpl_nLnwO28N0c_p-xtjPYOs2ViG2wYq4JIJNeCS66R2hiqeHvmYlab--O3JuT-DkhSUIsZGJuNZ0ZXabLE9iH6H6Or6cJL-fyrDFwGeNAgMBAAGjggHuMIIB6jAfBgNVHSMEGDAWgBQ901Cl1qCt7vNKYApl0yHU-PjWDzAdBgNVHQ4EFgQUakOQfTuYFHJSlTqqKApD-FF-06YwJQYDVR0RBB4wHIIKZ2l0aHViLmNvbYIOd3d3LmdpdGh1Yi5jb20wDgYDVR0PAQH_BAQDAgWgMB0GA1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjB1BgNVHR8EbjBsMDSgMqAwhi5odHRwOi8vY3JsMy5kaWdpY2VydC5jb20vc2hhMi1ldi1zZXJ2ZXItZzEuY3JsMDSgMqAwhi5odHRwOi8vY3JsNC5kaWdpY2VydC5jb20vc2hhMi1ldi1zZXJ2ZXItZzEuY3JsMEIGA1UdIAQ7MDkwNwYJYIZIAYb9bAIBMCowKAYIKwYBBQUHAgEWHGh0dHBzOi8vd3d3LmRpZ2ljZXJ0LmNvbS9DUFMwgYgGCCsGAQUFBwEBBHwwejAkBggrBgEFBQcwAYYYaHR0cDovL29jc3AuZGlnaWNlcnQuY29tMFIGCCsGAQUFBzAChkZodHRwOi8vY2FjZXJ0cy5kaWdpY2VydC5jb20vRGlnaUNlcnRTSEEyRXh0ZW5kZWRWYWxpZGF0aW9uU2VydmVyQ0EuY3J0MAwGA1UdEwEB_wQCMAAwDQYJKoZIhvcNAQELBQADggEBAG_nbcuC8--QhwnXDxUiLIz-06scipbbXRJd0XjAMbD_RciJ9wiYUhcfTEsgZGpt21DXEL5-q_4vgNipSlhBaYFyGQiDm5IQTmIte0ZwQ26jUxMf4pOmI1v3kj43FHU7uUskQS6lPUgND5nqHkKXxv6V2qtHmssrA9YNQMEK93ga2rWDpK21mUkgLviTPB5sPdE7IzprOCp-Ynpf3RcFddAkXb6NqJoQRPrStMrv19C1dqUmJRwIQdhkkqevff6IQDlhC8BIMKmCNK33cEYDfDWROtW7JNgBvBTwww8jO1gyug8SbGZ6bZ3k8OV8XX4C2NesiZcLYbc2n7B9O-63M2k>`_
- `A Layman's Guide to a Subset of ASN.1, BER, and DER <http://luca.ntop.org/Teaching/Appunti/asn1.html>`_

