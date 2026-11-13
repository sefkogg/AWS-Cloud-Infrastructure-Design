# Security architecture

## Security-group rules (illustrative)
| Group | Inbound | Outbound |
|---|---|---|
| ALB-SG | TCP 443 from `0.0.0.0/0` | TCP 8080 to APP-SG |
| APP-SG | TCP 8080 from ALB-SG | TCP 5432 to DB-SG; HTTPS 443 to approved destinations/endpoints |
| DB-SG | TCP 5432 from APP-SG | Minimal traffic required for the managed database |

Security groups are stateful. Do not expose SSH (22), RDP (3389), or PostgreSQL (5432) to the public internet. The application should validate requests and use TLS where supported for database connections.

## Identity and secrets
- Use IAM roles attached to EC2 rather than embedding access keys.
- Use Systems Manager Session Manager for administrative access; configure the required IAM and network connectivity.
- Store database credentials in AWS Secrets Manager and rotate them where supported.
- Restrict IAM permissions by role and review access periodically.
- Encrypt RDS storage and backups using AWS KMS.

## Logging and detection
- Enable CloudTrail for account activity and retain logs securely.
- Configure CloudWatch alarms for ALB target health, EC2 CPU, application errors, and database metrics.
- Consider AWS WAF on the ALB for common web attack filtering.
- Use VPC Flow Logs for network troubleshooting and investigations, with retention controls.

## Risk scenarios
| Risk | Mitigation |
|---|---|
| Public database exposure | Isolated subnets and DB-SG allowing only APP-SG |
| Compromised EC2 instance | Least-privilege instance role, patching, logging, no inbound SSH |
| Credential leakage | Secrets Manager and IAM role-based access |
| Malicious HTTP requests | HTTPS, app validation, optional AWS WAF |
| Single-AZ outage | Two-AZ app tier and RDS Multi-AZ |

These controls reduce risk but do not guarantee security.
