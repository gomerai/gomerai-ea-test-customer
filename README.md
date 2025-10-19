# gomerai-ea-test-customer

Test customer repository for EA (Enterprise Agent) onboarding, license automation, and bridge configuration testing.

## Overview

This repository simulates a customer's dedicated GitHub repository used during the gomerai onboarding process. It contains the necessary configuration and secrets for:

- **License verification and management**
- **EA (Enterprise Agent) configuration**
- **Bridge service connectivity testing**
- **Automated license validation workflows**

## Repository Structure

```
gomerai-ea-test-customer/
├── .github/
│   └── workflows/
│       └── ea-license-verification.yml   # Automated license verification workflow
└── README.md                              # This file
```

## Repository Secrets

The following secrets are configured in Settings > Secrets and variables > Actions:

### License & Customer Information

- **`LICENSE_TOKEN`** - Authentication token for license verification
  - Used to validate the customer's license with the gomerai licensing system
  - Stored in the 10web database and synced to GH Secrets at onboarding

- **`GomerAI_ID`** - Unique identifier for this customer
  - Example: `test_customer_001`
  - Used to identify the customer in all API calls and logging

- **`TENANT_ID`** - Multi-tenant identifier
  - Example: `tenant_gomerai_test_001`
  - Used for tenant isolation in multi-customer environments

### EA Bridge Configuration

- **`EA_BRIDGE_URL`** - URL endpoint for the EA Bridge service
  - Example: `https://bridge.gomerai.example.com/api/v1`
  - The bridge service connects the EA to Google Cloud (GC)

- **`EA_API_KEY`** - Authentication key for EA to connect to the bridge
  - Rotated periodically for security
  - Allows EA to authenticate and send data to the bridge

## Workflow: EA License Verification

The `.github/workflows/ea-license-verification.yml` workflow:

1. **Triggers**: Runs on push to main branch, and scheduled (daily at 6 AM UTC)
2. **Purpose**: Verifies the customer's license is valid and active
3. **Process**:
   - Fetches license status from 10web using `LICENSE_TOKEN` and `GomerAI_ID`
   - Validates license is not expired or revoked
   - Tests connectivity to EA Bridge using `EA_BRIDGE_URL` and `EA_API_KEY`
   - Reports success or failure

### Workflow Environment Variables

All secrets are injected as environment variables during workflow execution:

```yaml
env:
  LICENSE_TOKEN: ${{ secrets.LICENSE_TOKEN }}
  GomerAI_ID: ${{ secrets.GomerAI_ID }}
  TENANT_ID: ${{ secrets.TENANT_ID }}
  EA_BRIDGE_URL: ${{ secrets.EA_BRIDGE_URL }}
  EA_API_KEY: ${{ secrets.EA_API_KEY }}
```

## Security Notes

- 🔒 All secrets are stored securely in GitHub Secrets (encrypted at rest)
- 🔒 Secrets are only accessible to GitHub Actions workflows
- 🔒 Never commit secrets to the repository code
- 🔒 Per-customer secrets are generated during onboarding
- 🔒 Licensing credentials are stored in the 10web database
- 🔒 EA retrieves secrets from GH Secrets or GC Secrets Manager

## Troubleshooting

### License Verification Fails

- Verify `LICENSE_TOKEN` matches the token in 10web database
- Check if license is active and not expired
- Confirm `GomerAI_ID` is correct

### Bridge Connection Fails

- Verify `EA_BRIDGE_URL` is accessible
- Check `EA_API_KEY` is valid and not revoked
- Ensure bridge service is running and healthy

### Workflow Errors

- Review Actions logs for specific error messages
- Verify all 5 secrets are configured in repository settings
- Check network connectivity to bridge URL

## Related Resources

- [GitHub Secrets Documentation](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- [GitHub Actions Workflows](https://docs.github.com/en/actions/using-workflows)
- gomerai EA Documentation (internal)
- gomerai Bridge API Documentation (internal)

## Support

For issues with this test customer repository:

- Check the Actions workflow logs
- Verify all secrets are properly configured
- Contact gomerai support team

---

**Repository Purpose:** Test and validation environment for customer onboarding workflow  
**Last Updated:** October 2025  
**Status:** ✅ Active - All secrets configured
