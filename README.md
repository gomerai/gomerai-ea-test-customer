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

- **`CUSTOMER_ID`** - Unique identifier for this customer
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
  - Format: `sk_test_ea_api_key_*`
  - Required for all EA → Bridge → GC communications

## GitHub Actions Workflow

### EA License Verification and Configuration Test

**File:** `.github/workflows/ea-license-verification.yml`

**Triggers:**
- Manual workflow dispatch
- Push to `main` branch
- Daily scheduled run (midnight UTC)

**Purpose:**
- Verifies that all required secrets are configured
- Tests connectivity to the EA Bridge
- Validates license tokens
- Ensures the EA can authenticate and communicate

**Usage:**
Navigate to Actions tab and manually trigger the workflow to test configuration.

## Data Flow Architecture

```
EA (Enterprise Agent)
  ↓
  ↓ [Authenticated with EA_API_KEY]
  ↓
EA Bridge Service
  ↓
  ↓ [License verified with LICENSE_TOKEN]
  ↓
Google Cloud (GC)
```

**Requirements for data flow:**
1. ✅ Valid `LICENSE_TOKEN` (verified against 10web database)
2. ✅ Valid `EA_API_KEY` (for EA → Bridge authentication)
3. ✅ Accessible `EA_BRIDGE_URL` (bridge must be operational)
4. ✅ Proper `CUSTOMER_ID` and `TENANT_ID` configuration

## Testing This Repository

### 1. Verify Secrets are Configured

```bash
# All secrets should be visible in:
# Settings > Secrets and variables > Actions > Repository secrets
```

### 2. Run the Workflow

1. Go to the **Actions** tab
2. Select "EA License Verification and Configuration Test"
3. Click "Run workflow"
4. Monitor the workflow execution

### 3. Expected Workflow Output

```
✓ LICENSE_TOKEN configured
✓ CUSTOMER_ID configured
✓ TENANT_ID configured
✓ EA_BRIDGE_URL configured
✓ EA_API_KEY configured
Ready for EA data flow through bridge to GC
```

## Customer Onboarding Process

This repository represents Step 3 in the customer onboarding workflow:

1. **Customer Signs Up** - New customer account created
2. **License Generated** - License token created in 10web database
3. **GitHub Repository Created** ⬅️ *You are here*
   - Dedicated repo created for customer
   - Repository secrets populated from secure sources
4. **EA Configured** - Enterprise Agent configured with secrets
5. **Bridge Connection Tested** - Verify EA can connect to bridge
6. **License Verified** - Confirm licensing is valid
7. **Data Flow Enabled** - Customer can send data to GC

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
- Confirm `CUSTOMER_ID` is correct

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
