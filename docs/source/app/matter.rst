Matter
##########



背景
========

智能家居，近场控制。

以 ipv6 over BLE/WIFI/Thread 为基础，设定应用层协议。

init: PASE
==============

PASE: 以 spake2+ 做 device 初始化配对，administrator （例如手机）为commissioner，device为commissionee。

配对完毕，建立安全信道。

commissionee发device attestation，commissioner进行校验。

校验通过，commissioner向device发nocsr request，device提交nocsr之后，commissioner为device签发node operation certificate (noc)。

每个matter device可以有多个noc。

comm: CASE
============

CASE: node 之间，基于 noc 进行 sigma-i 协商，安全通信。

这里sigma-i的中间key又基于session context派生，与tls类似。

acl
=====

administrator 与 受控 node  通信，更新acl。

administrator 与 控制 node 通信，更新noc。

node之间的控制通信，会检查noc里的标识是否与acl匹配。

resumption
============

除了实时的sigma-i，还支持基于sigma-i shared_secret的resumption，主要是结合context做salt派生，比较简单。

group key
============

administrator 安全分发 group key，这块主要follow Thread的设计(epoch key轮转更新，基于当前版本的epoch key派生group key)，没啥特别的。


参考资料
===========

- `matter <https://github.com/project-chip/connectedhomeip>`_

