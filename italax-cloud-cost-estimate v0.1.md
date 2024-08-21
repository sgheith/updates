# Italax.Cloud Deployment - Monthly Cost Estimate (Upper Band)

## Overview
This cost estimate represents an upper band scenario for the first database deployment model, utilizing a single VM for reporting and a single container for the ASP.NET Core application and SQL Express database.

## Important Note on Deployment Strategy
While this estimate provides an upper band for budgeting purposes, it's recommended to start the deployment with lower specifications. As the system goes live, closely monitor customer usage patterns and the number of provisioned customers. Based on these observations, gradually adjust and scale the resources to match actual needs, which may result in lower initial costs and more efficient resource utilization. Also this estimate assumes a Part-time DevOps Engineer to maintain and monitor the system, his cost is very rough.

## 1. Virtual Machine for Reporting

- VM Type: Azure D4s v3 (4 vCPUs, 16 GB RAM)
- Estimated Cost: $175/month

## 2. Container Instance

- CPU: 32 vCPUs
- RAM: 64 GB
- Usage: 5 hours/day, 5 days/week (100 hours/month)
- Note: 5 hours per day is a high estimate, representing peak utilization periods
- Estimated Cost: $400/month

## 3. Storage

- Volume for Databases: 2.5 TB (500 organizations * 5 GB)
- Backup Storage: 2.5 TB
- Estimated Cost: $100/month

## 4. Backup Solutions

- Azure Backup Service
- Estimated Cost: $80/month

## 5. SSL Certificate

- Standard SSL Certificate
- Estimated Cost: $10/month (amortized annual cost)

## 6. Administration and Monitoring

- Part-time DevOps Engineer
- Estimated Cost: $500/month

## 7. Miscellaneous

- Networking costs, small buffer for unexpected expenses
- Estimated Cost: $80/month

## Total Estimated Monthly Cost (Upper Band)

Summing up the above components:

$175 + $400 + $100 + $80 + $10 + $500 + $80 = $1,345/month

## Notes

- This estimate represents an upper band based on the first database deployment model (VM & Single Container).
- Initial deployment should start with lower specifications and scale up as needed based on actual usage patterns and customer growth.
- The VM specifications (Azure D4s v3) and Container specifications (32 vCPUs, 64 GB RAM) represent maximum anticipated needs.
- Database size per organization is set at 5 GB, but actual usage may vary.
- The 5-hour daily usage estimate is intentionally high, representing periods of maximum utilization.
- Actual costs may be significantly lower depending on real usage patterns and optimization efforts.
- Azure offers various pricing models and discounts for long-term commitments, which could potentially reduce overall costs.
- Other cloud providers may provide cheaper resources.
- Regular monitoring and adjustment of resources is crucial for cost optimization and performance tuning.
- A rough Part-time DevOps Engineer cost is added but actual cost may be very different.