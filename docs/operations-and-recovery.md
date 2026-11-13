# Operations, monitoring, backup, and recovery

## Monitoring plan
- ALB: unhealthy target count, 5xx responses, response time.
- EC2: CPU, status checks, application logs, memory (with CloudWatch agent).
- RDS: CPU, storage, connections, failover events, backup status.
- Alerting: CloudWatch alarms routed through Amazon SNS to an operations email endpoint.

## Backup plan
- Enable RDS automated backups with a defined retention period (example: 7 days).
- Define AWS Backup policies for supported resources, with encryption and access controls.
- Store application configuration in version control; keep secrets out of Git.
- Document recovery point objective (RPO) and recovery time objective (RTO) as business requirements before deployment. No numeric RPO/RTO is claimed here.

## Recovery scenarios
**App instance failure:** ALB removes unhealthy targets; Auto Scaling replaces unhealthy instances. Confirm sufficient capacity in the remaining AZ.

**AZ failure:** ALB routes to healthy targets in surviving AZ. Multi-AZ RDS can fail over to its standby, with a temporary interruption possible.

**Accidental database deletion or corruption:** Identify the incident, stop harmful writes, restore to a new RDS instance using an appropriate backup or point-in-time restore, validate data, and update application connection settings.

## Operational checklist
1. Confirm CloudWatch alarms and notification delivery.
2. Review patching, certificate renewal, and secret rotation procedures.
3. Run restore exercises periodically and record outcomes.
4. Review costs and access permissions monthly.
5. Document incident response ownership and escalation.
