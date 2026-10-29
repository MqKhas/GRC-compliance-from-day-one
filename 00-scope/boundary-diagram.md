# Boundary & Architecture Diagram

As of 2026-10-29, system version `tracker@9124656`. Dashed boxes are out of scope or carved out. See section 4 of the [system description](system-description.md) for the reasons.

```mermaid
flowchart TB
  subgraph USERS["Users (out of scope: covered by CUECs)"]
    direction LR
    phone["User's phone<br/>PWA + local encrypted cache"]
    mail["User's mailbox"]
    authapp["Authenticator app"]
  end

  subgraph ADMIN["Administrator identities (in scope)"]
    direction LR
    owner["System owner"]
    apple["Apple Account<br/>password manager"]
    google["Google account<br/>GitHub sign-in, admin email"]
  end

  subgraph GH["GitHub (in scope, carved-out vendor)"]
    repo["Private app repository"]
    actions["Actions: test + cdk deploy"]
  end

  subgraph AWS["AWS account (in scope, carved-out vendor)"]
    subgraph EDGE["Edge"]
      r53["Route 53<br/>domain + DNS"]
      cf["CloudFront<br/>HTTPS + security headers"]
      acm["ACM certificate"]
    end
    subgraph APP["Application (us-east-1)"]
      s3["S3 bucket<br/>static app files, private"]
      cognito["Cognito user pool<br/>invite-only, TOTP required"]
      apigw["API Gateway HTTP API<br/>JWT authorizer, throttled"]
      lambda["Lambda sync function"]
      ddb[("DynamoDB<br/>ciphertext records")]
      ses["SES<br/>invite + code emails"]
      logs["CloudWatch Logs<br/>14 days"]
    end
    subgraph RES["Resilience and cost"]
      backup["AWS Backup<br/>35 days"]
      copy[("Backup copy, us-west-2<br/>3 years")]
      budget["Budgets alerts"]
    end
    iam["IAM: admin user + OIDC deploy role"]
    cshell["CloudShell<br/>accounts.sh"]
    personal["Personal website<br/>(carved out, D-007)"]:::out
  end

  phone -- "HTTPS: app files" --> cf
  phone -- "HTTPS: sign-in" --> cognito
  phone -- "HTTPS /api + JWT" --> cf
  cf --> s3
  cf -- "/api/*" --> apigw
  apigw -- "validates JWT from" --> cognito
  apigw --> lambda
  lambda --> ddb
  lambda --> logs
  ddb --> backup --> copy
  cognito --> ses --> mail
  authapp -. "6-digit code" .-> phone

  owner --> apple
  owner --> google
  google -- "sign in" --> repo
  repo -- "push to main" --> actions
  actions -- "OIDC, main only" --> iam
  apple -- "console sign-in + MFA" --> iam
  iam --> cshell
  cshell -- "create / delete users" --> cognito
  budget --> google
  r53 --> cf
  acm --> cf

  classDef out stroke-dasharray: 5 5,color:#888;
```

## Reading the diagram

- **Two paths reach production.** Code reaches production through Google → GitHub → Actions → the OIDC role. Account administration goes through the administrator's stored credentials → IAM → CloudShell. Both admin identities are therefore in scope (D-008, proposed).
- **The personal website is carved out**, but it sits inside the same AWS account. Account-level controls (root, admin, billing, DNS) cover both, so they stay in scope (D-007).
- **Users' devices are out of scope**, but the system relies on users protecting them. Those duties are listed as CUECs in the system description.
