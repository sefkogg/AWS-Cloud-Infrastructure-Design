# Deployment validation checklist (not yet executed)

These are **proposed tests**, not completed test results.

- [ ] VPC and six subnet CIDRs match the design; no overlaps.
- [ ] Public subnet route tables use an Internet Gateway.
- [ ] Private app subnet default routes point to NAT gateways in their own AZs.
- [ ] Database subnets have no internet default route.
- [ ] ALB HTTPS listener uses a valid ACM certificate.
- [ ] HTTP-to-HTTPS redirect is configured if port 80 is exposed.
- [ ] EC2 instances have no public IP addresses.
- [ ] Only ALB-SG can reach APP-SG on TCP 8080.
- [ ] Only APP-SG can reach DB-SG on TCP 5432.
- [ ] RDS is not publicly accessible.
- [ ] Both AZs have healthy app targets.
- [ ] CloudWatch alerts deliver to the intended recipient.
- [ ] Backup retention and a test restore are verified.
- [ ] Simulated instance failure does not prevent service from recovering.
- [ ] Failover procedure is tested and actual recovery time recorded.
- [ ] AWS Budget alerts are configured before any deployment.
