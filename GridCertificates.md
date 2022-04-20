# wLCG Grid Certificates

## Background

For many many years, the German GridKa Certification Authority (CA) at KIT in Karlsruhe will cease operation at 11 June 2023 as the CA cert ends. 
As a successor the [GÉANT](https://geant.org/) Europe's leading collaboration on network and related infrastructure and services for the benefit of research and education offers the services Sectigo Certificate Manager to obtain Grid user certificates starting now.

## Introduction

On order to access global Grid resources, users must hold a valid personal Grid user certificate (authentication) AND users must be member of a Virtual Organization (VO) (authorization).
A valid Grid user certificate is a prerequisite to request membership in a VO. Users usually have one Grid user certificate. Multiple VO membership is possible.[^note]

A Grid user certificate (format X509) consists of a private key with a private password and a certified public key. The private key and the password is exclusively possessed by the user and is NOT known to the Registration Authority (RA) or Certification Authority (CA) at any stage.

A certificate is valid for one (1) year and can be renewed. Users get notified by the CA via email three (3) weeks before the expiration date. It is strongly recommended to renew the certificate before its expiration.

The certificate can/should be copied to all devices/browsers which need it.

[^note]: A Grid user certificate can be seen as an analogy to a passport, whereas the VO membership compares to a visa.

## Authorities

The [GÉANT](https://geant.org/) CA is part of The [International Grid Trust Federation (IGTF)](http://www.igtf.net/) hence Grid user certificates are accepted by all Grid sites in WLCG. In order to facilitate the request procedure, many institutions in Germany operated Registration Authorities (RA) which take over the necessary paper-work on behalf of the CA.

BUW employees may use their ZIM account to authenticate against GÉANT and request a Grid user certificate. Use the portal [Certificate Manager SSO Check](https://cert-manager.com/customer/DFN/ssocheck/) to test your account. 

Non-BUW users can not use the portal and should rather check with their home institution officials how to proceed.

## Procedure

1. Test your credentials (account_name / password) in [Certificate Manager SSO Check](https://cert-manager.com/customer/DFN/ssocheck/). If you encounter problems during the certificate request, please screenshot your status information in the SSO check portal for further debugging
2. Request a Grid user certificate in [GEANT User Cert](https://cert-manager.com/customer/DFN/idp/clientgeant).
   - profile: GÉANT IGTF-MICS Personal
    - chose: Key Generation
    - algorithm: RSA-4096
    - more detailed information is available on the [DFN FAQ page](https://doku.tid.dfn.de/de:dfnpki:tcsfaq)
3. [VOMS](https://lcg-voms2.cern.ch:8443/voms/atlas/user/home.action)
