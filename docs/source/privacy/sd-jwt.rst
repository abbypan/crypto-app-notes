SD-JWT
=========

`Selective Disclosure for JWTs (SD-JWT) <https://datatracker.ietf.org/doc/draft-ietf-oauth-selective-disclosure-jwt/>`_

基于Issuer-signed JWT的selective disclosure，普通jwt结构。

holder的proof key为optional，称为key binding，置于claim的cnf (Confirmation Key)。

key binding jwt的claim包含Issuer-signed JWT。

cmtList型的disclosured，需要salt。

unlinkable需要过issuer。

predicate支持可以考虑改造Hash。
