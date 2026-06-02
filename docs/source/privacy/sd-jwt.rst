SD-JWT
=========

`Selective Disclosure for JWTs (SD-JWT) <https://datatracker.ietf.org/doc/draft-ietf-oauth-selective-disclosure-jwt/>`_

基于Issuer-signed JWT的selective disclosure，普通jwt结构。

holder的proof key为optional，称为key binding，置于claim的cnf (Confirmation Key)。

key binding jwt的claim包含Issuer-signed JWT。


attr 
-------

cmtList型的disclosured，需要salt。

        [salt, attr name, attr value]  ->  attr hash


trace
---------

can not hide holder public key.

unlinkable需要实时request issuer signing.

predicate例如age>18, country=cn等支持，需以static value形式验hash，不是range proof.

