# Network and subnet plan

## Address allocation
VPC CIDR: `10.20.0.0/16` (65,536 IPv4 addresses in the block; AWS reserves five IP addresses in each subnet).

| Subnet | CIDR | AZ | Routing purpose |
|---|---|---|---|
| Public A | 10.20.0.0/24 | A | ALB interface and NAT gateway A |
| Public B | 10.20.1.0/24 | B | ALB interface and NAT gateway B |
| Private app A | 10.20.10.0/24 | A | Application EC2 instances |
| Private app B | 10.20.11.0/24 | B | Application EC2 instances |
| Isolated database A | 10.20.20.0/24 | A | RDS subnet group |
| Isolated database B | 10.20.21.0/24 | B | RDS subnet group |

## Route tables
- **Public A and B:** local VPC route plus `0.0.0.0/0 → Internet Gateway`.
- **Private app A:** local route plus `0.0.0.0/0 → NAT Gateway A`.
- **Private app B:** local route plus `0.0.0.0/0 → NAT Gateway B`.
- **Database A and B:** local VPC route only; no default internet route.

## Request flow
1. Client resolves the application hostname via Route 53.
2. Client connects to the internet-facing ALB on TCP 443; ACM provides the TLS certificate.
3. ALB forwards to healthy application targets on TCP 8080 in private app subnets.
4. Application servers connect to RDS on TCP 5432 in isolated database subnets.
5. App instances send telemetry to CloudWatch and receive updates using NAT or configured VPC endpoints.

## Resilience note
Each AZ has its own NAT gateway so private application egress does not depend on the other AZ. ALB targets and application instances span both AZs. Actual resilience also depends on application statelessness, capacity, and dependencies.
