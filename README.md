# APIVerve DNS Monitor Action

> Verify DNS configuration, check propagation, and validate DNSSEC after deployments

> **Beta Release** - This action is in beta. We'd love your feedback! [Open an issue](https://github.com/apiverve/action-dns-monitor/issues) if you encounter any problems.

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-DNS Monitor-blue?logo=github)](https://github.com/marketplace/actions/apiverve-dns-monitor)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**[Browse All APIs](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=dns-monitor)** | **[Get Free API Key](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=dns-monitor)** | **[Documentation](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=dns-monitor)**

---

## What does this action do?

This action provides access to APIVerve's DNS Monitor APIs directly in your GitHub workflows:

- Verify DNS records after infrastructure changes
- Check DNS propagation across global servers
- Validate DNSSEC configuration
- Monitor MX records for email deliverability

### Available APIs

| API | Description |
|-----|-------------|
| `dnslookup` | DNS Lookup is a simple tool for looking up the DNS records of a domain. It returns the A, MX, and other records of the domain. |
| `dnspropagation` | DNS Propagation Checker verifies if DNS changes have propagated across multiple global DNS servers. It queries DNS servers worldwide to show the current state of your DNS records. |
| `dnsseccheck` | DNSSEC Checker validates the DNSSEC (Domain Name System Security Extensions) configuration of a domain. It verifies that DNS responses are authenticated and haven&#x27;t been tampered with. |
| `nameservers` | Nameservers is a tool for looking up the authoritative nameservers for any domain. Returns nameserver hostnames, IP addresses, reverse DNS, and owner information. |
| `mxlookup` | MX Lookup is a simple tool for getting MX records for a domain. It returns the MX records for the given domain. |

---

## Quick Start

```yaml
- name: DNS Monitor
  uses: apiverve/action-dns-monitor@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: dnslookup
    params: '{&quot;domain&quot;: &quot;example.com&quot;, &quot;type&quot;: &quot;A&quot;}'
```

---

## Setup

### 1. Get Your API Key

Sign up for a free account at [dashboard.apiverve.com/signup](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=dns-monitor) and create an API key.

### 2. Add Secret to Repository

Go to your repository **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

- Name: `APIVERVE_KEY`
- Value: Your API key from the dashboard

### 3. Use in Workflow

```yaml
- name: DNS Monitor
  uses: apiverve/action-dns-monitor@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: dnslookup
    params: '{"your": "parameters"}'
```

---

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api_key` | Your APIVerve API key (or set `APIVERVE_API_KEY` env var) | Yes* | - |
| `api` | API to use: `dnslookup`, `dnspropagation`, `dnsseccheck`, `nameservers`, `mxlookup` | No | `dnslookup` |
| `params` | JSON parameters for the API | No | `{}` |
| `output_file` | Path to save binary output (images, PDFs) | No | - |
| `format` | Response format: `json`, `yaml`, or `xml` | No | `json` |
| `fail_on_error` | Fail workflow if API returns error | No | `true` |

*\*API key is required but can be provided via input OR `APIVERVE_API_KEY` / `APIVERVE_KEY` environment variable.*

## Outputs

| Output | Description |
|--------|-------------|
| `result` | Full API response as JSON |
| `data` | The `data` field from response as JSON |
| `status` | API status (`ok` or `error`) |
| `file` | Path to downloaded file (if `output_file` was used) |

---

## Examples

### DNS Lookup

Check DNS records for a domain

```yaml
- name: DNS Lookup
  id: dns-monitor-0
  uses: apiverve/action-dns-monitor@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: dnslookup
    params: '{&quot;domain&quot;: &quot;example.com&quot;, &quot;type&quot;: &quot;A&quot;}'

- name: Use result
  run: echo "Result: ${{ steps.dns-monitor-0.outputs.data }}"
```

### DNS Propagation

Check if DNS changes have propagated globally

```yaml
- name: DNS Propagation
  id: dns-monitor-1
  uses: apiverve/action-dns-monitor@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: dnspropagation
    params: '{&quot;domain&quot;: &quot;api.example.com&quot;, &quot;type&quot;: &quot;A&quot;}'

- name: Use result
  run: echo "Result: ${{ steps.dns-monitor-1.outputs.data }}"
```

### DNSSEC Validation

Verify DNSSEC is properly configured

```yaml
- name: DNSSEC Validation
  id: dns-monitor-2
  uses: apiverve/action-dns-monitor@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: dnsseccheck
    params: '{&quot;domain&quot;: &quot;example.com&quot;}'

- name: Use result
  run: echo "Result: ${{ steps.dns-monitor-2.outputs.data }}"
```


---

## Full Workflow Example

```yaml
name: DNS Monitor Workflow

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  dns-monitor:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run DNS Monitor
        id: result
        uses: apiverve/action-dns-monitor@v1
        with:
          api_key: ${{ secrets.APIVERVE_KEY }}
          api: dnslookup
          params: '{&quot;domain&quot;: &quot;example.com&quot;, &quot;type&quot;: &quot;A&quot;}'

      - name: Show result
        run: |
          echo "Status: ${{ steps.result.outputs.status }}"
          echo "Data: ${{ steps.result.outputs.data }}"
```

---

## Related Actions

Looking for more APIVerve actions?

- [apiverve/action](https://github.com/apiverve/action) - Generic action for all 350+ APIs
- [apiverve/action-release-assets](https://github.com/apiverve/action-release-assets) - Generate QR codes, barcodes, and badges for your GitHub releases
- [apiverve/action-visual-testing](https://github.com/apiverve/action-visual-testing) - Capture screenshots and generate PDFs for visual regression testing and documentation
- [apiverve/action-domain-health](https://github.com/apiverve/action-domain-health) - Monitor domain expiration, WHOIS changes, and domain availability

**[Browse all APIVerve Actions →](https://github.com/marketplace?query=apiverve)**

---

## Pricing

- **Free tier** - Get started with generous free limits
- **Pro plans** - Higher rate limits and priority support for production use

Check out [pricing details](https://apiverve.com/pricing?utm_source=github&utm_medium=action&utm_campaign=dns-monitor).

---

## Resources

- **API Documentation**: [docs.apiverve.com](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=dns-monitor)
- **API Marketplace**: [apiverve.com/marketplace](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=dns-monitor)
- **Issues & Support**: [GitHub Issues](https://github.com/apiverve/action-dns-monitor/issues)
- **Email**: support@apiverve.com

---

## License

MIT - see [LICENSE](LICENSE)

---

Built by [APIVerve](https://apiverve.com?utm_source=github&utm_medium=action&utm_campaign=dns-monitor) - 350+ APIs for developers
