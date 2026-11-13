# Cost and design tradeoffs

This design prioritizes availability and isolation over minimum cost. **No prices are quoted** because AWS pricing depends on region, usage, and configuration.

## Main cost drivers
- Two NAT gateways and processed traffic.
- Application Load Balancer hours and load balancer capacity units.
- EC2 instance hours, storage, and data transfer.
- RDS Multi-AZ instance/storage/backup usage.
- CloudWatch log ingestion and retention.
- Route 53, AWS Backup, Secrets Manager, and other optional services.

## Tradeoffs
| Decision | Benefit | Cost/complexity |
|---|---|---|
| Two AZs | Better fault tolerance | More infrastructure and data transfer |
| NAT gateway per AZ | Independent app egress | Recurring gateway and processing charges |
| RDS Multi-AZ | Managed failover | Higher database cost |
| Private app tier | Smaller direct attack surface | Additional routing and management design |
| CloudWatch + CloudTrail | Visibility and audit trail | Storage, retention, and alert configuration |

## Lower-cost learning variant
For a learning deployment, use a single-AZ environment with small resources, no NAT gateway where endpoints can meet needs, and a limited lifetime. **This sacrifices high availability** and should not be presented as equivalent to the reference design. Set an AWS Budget alert before creating billable resources.

Use the [AWS Pricing Calculator](https://calculator.aws/) to estimate a deployment for a selected region and workload before provisioning.
