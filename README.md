Here in can be found the beginnings of some tools for producing DNSec.

gentlsa.py:
Outputs an example TLSA record for a given public key.

chain.py:
Generates a DNSec chain. For example:
% python chain.py www.example.org chain

gencert.c:
Builds a self-signed certificate with an embedded chain. For example:
% ./gencert key.pem chain

**Proctectiong Ilustration
Attacking Ilustration on DNSec - Image by DNSec Team
![alt text](https://lh3.googleusercontent.com/-s60hz4H30UEk_1MMPtcGCiTsUkf4ua9WmFcyJuxwtHPFvz4OzLFvp5kDeCKUf80aQAJZbCPA_OYoojxBNqnC0tnYc5DyINUrxYoIvwDBGq9ASlaDVmHaJdK4HJFZVEP_vW3kOqOn_noEXbO3TMOM_T1HNMMM1qYjC845C86uefbOG21kXw-TD92jFf4juwlgo8dHotPdUdJxRMr0TwlHKpM43BbEm-8eH5Aj2j0FwCiSyj7Bsu5h9oNcTfWTAx-5DGcxlao-59d9hHkKOBrKhvn6H5rU-iDEmw-p4gepbn5c7GfL7xKEBxPmaWjYV6hHcgq5g0eb8gfi9jCw_Yv_1kgjgmgzUuDIb_iRHy6-oA4eboi6C7HPd2Vt2yHT6uuQWOvwWqBLwBkhkuYsh-nXYL3prfPAIQjAwRZpMuvrYgHO1s6_Onm_4uBlJ2cH0Yue616PLCVd8Xja3n_gaZo_jDC2REF4S1qI-qN1Q5onmr3EEQ7ElOE-K_eqYRonB_0c8bs0Mm1RScROvZNu2i4oWPxNS8Sab7wWIFFdBh6I0RVOtjWek94rf-PRRfYbSD7JIHvkfYoSgywh919Ef2BOHolt2CuuwEXoVYn0HVNiP2EAoLzyeMmRny7Nik-RDIWyulY2N4yam2RstB3SG1Xvwk=w882-h443-no "Attacking Ilustration on DNSec - Image by DNSec Team")


Example
-------

    $ openssl genrsa 1024 > privkey.pem
    $ openssl rsa -pubout -in privkey.pem > pubkey.pem
    $ python ./gentlsa.py pubkey.pem
    Result Show -> _443._tcp.EXAMPLE.COM. 60 IN TYPE52 \# 35 020461757468303e3039060a2b06010401d67902

(Put this in your DNS zone, but don't forget to change "EXAMPLE.COM." to match the actual domain name. Once this is done, and the record is public, you can do the next step. You can check the record with `dig -t type52 example.com`.)

    $ python ./chain.py example.com chain
(Don't forget to change example.com to the actual domain name in your server.)

    $ gcc -o gencert gencert.c -Wall -lcrypto
    $ ./gencert privkey.pem chain > cert.pem

(And, to check the certificate:)

    $ openssl x509 -text < cert.pem | less
    when your open this file is file cant open

