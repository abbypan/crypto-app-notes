Secure CA operation
#######################

`BSI TR-03145 Secure Certification Authority operation <https://www.bsi.bund.de/EN/Service-Navi/Publications/TechnicalGuidelines/TR03145/TechnicalGuidelines_03145_node.html>`_

文档结构:
- Introduction, scope
- Abbreviation
- Concept and approach: 图~
- Aims of the CA
- Processes of the CA
- General security requirements


abbr
==========================================================

certificate policy (CP): 一些命名规则，标识common security requirement, RFC3647

certification practice statement (CPS): CA对签发、管理、撤销、续期的实际操作策略

Certification Authority as trustee in PKI
==========================================================

目标(security objectives):
- Trustworthy CA certificate: (Depends on)
  - Private key of the CA
  - Certificate generation process
- Provision of trust services to relying parties: (Depends on)
  - dissemination of trustable certificates

Certificate Management Processes
==========================================================

Private key of the CA
----------------------------------------------------

Maintaining: (process)
- Certificate generation process
- Trustworthy CA certificate

Trustworthy Manner: (Depends on)
- General security requirements

Objectives -> Treats -> Requirements

Rationale:
- Objective -> Requirements -> Rationale
- Treat -> Requirements -> Rationale

