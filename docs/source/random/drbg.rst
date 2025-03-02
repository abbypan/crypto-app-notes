DRBG
#######

NIST SP 800-90A DRBG

`NIST SP <https://csrc.nist.gov/publications/sp>`_

`Recommendation for Random Number Generation Using Deterministic Random Bit Generators <https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf>`_

non-deterministic random bit generators (NRBGs)  纯物理实现，不可预测

Deterministic Random Bit Generators (DRBGs) 使用算法生成

概要
==========================================================

初始化，reseed，生成随机数

.. note::

    { internal state value, entropy input, additional input (optional) } => seed

    initial_working_state = Instantiate_algorithm ( entropy_input , nonce , personalization_string, security_strength).
    
    DRBG Instantiation : initialize with seed1 -> reseed with seed2 -> ...

    Reseed_function ( state_handle, prediction_resistance_request, additional_input )

    prediction_resistance_request:  ( status , entropy_input) = Get_entropy_input ( security_strength , min_length , max_length)

    Generate_function ( state_handle, requested_number_of_bits, requested_security_strength , prediction_resistance_request, additional_input)

类型
==========================================================

Hash_DRBG : hash函数用于instantiate, reseed and generate functions; 注意每次只使用同一种hash函数。

HMAC_DRBG : HMAC函数用于instantiate, reseed and generate functions; 注意有一个HMAC_DRBG_Update更新K, V；而K, V是用于hmac生成随机数的输入参数。

DRBG Mechanism Based on Block Ciphers

CTR_DRBG  uses an approved block cipher algorithm in the counter mode，注意CTR_DRBG也有CTR_DRBG_Update更新K，V。而K，V是用于aes之类的算法生成随机数的输入参数。

Dual_EC_DRBG 由于后门问题，业界不推荐使用。
