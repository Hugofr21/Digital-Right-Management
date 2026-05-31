# Digital-Right-Management

To assemble a complete DRM (crypto-protection, key management, and license management) solution and organize everything into versioned repositories, with ready-to-use documentation so that the team – and new developers – can understand, compile, test, and operate the solution.

## Overview

```mermaid
flowchart TD
    U[User] -->|requests the video| P[Media Player]
    P -->|loads manifest and detects encryption| M[Manifest CENC]
    M -->|passes to| E[EME - Encrypted Media Extensions]
    E -->|instantiates interface in OS| C[CDM - Interface OS/Browser]
    C -->|requests secure processing| TEE[TEE - Trusted Execution Environment]
    TEE -->|creates signed challenge with Device Key| D[Cryptographic Challenge]
    D -->|sends to| LS[License Server]
    LS -->|validates challenge and returns| L[Encrypted License]
    L -->|forwarded intact to| TEE
    TEE -->|extracts and retains the Content Key| CK[(Content Key isolated in Hardware)]
    P -->|sends encrypted video segments| S[Encrypted video segments]
    S -->|are injected into| TEE
    TEE -->|decrypts using the Content Key generating| F[Plaintext Video Frames]
    F -->|output direct via DMA| SV[Secure Video Path / HDCP]
    SV -->|renders content to| U

    subgraph INFRA [Backend Infrastructure]
        CDN[CDN - S3 + CloudFront]
        AUTH[Auth Service - Keycloak/OIDC]
        KMS[Key Management Service - AWS KMS / Vault]
    end

    LS -.->|queries keys| KMS
    LS -.->|validates identity| AUTH
    CDN -.->|delivers| M
    CDN -.->|delivers| S
```


## CDN (Content Delivery Network)

## License Server

## EME (Encrypted Media Extensions)

## CDM (Content Decryption Module) & TEE (Trusted Execution Environment)

## Output Protection / HDCP
