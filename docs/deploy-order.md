# Deployment Order

## Prereqs
- AWS account
- Terraform >= 1.6
- AWS CLI authenticated
- S3 + DynamoDB for remote state (or use local state for demo)

## Apply order
1. 10-foundation
2. 20-eks
3. 30-data
4. 40-app (build/push + kubectl apply)
5. 50-observability-security
