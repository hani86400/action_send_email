 # GitHub Actions Collection

Reusable GitHub Actions for automation, reporting, and notifications.

---

# Available Actions

| Action | Description |
|---|---|
| [hello](./hello/README.md) | Print a message multiple times |
| [cloudflare](./cloudflare/README.md) | Manage CloudFlare DNS records using API |
| [repository_report](./repository_report/README.md) | Generate repository reports |
| [send_email](./send_email/README.md) | Send email using API |

---

# Usage

Example:

```yaml
steps:

  - name: hello
    uses: hani86400/actions/hello@main
    with:
      MSG: "Hani"
      COUNT: 3


name: TEST_CLOUDFLARE_ADD

on:
  workflow_dispatch:

jobs:

  TEST:
    runs-on: ubuntu-latest

    steps:

      - name: ADD_RECORD
        uses: hani86400/actions/cloudflare@main
        with:
          API_TOKEN: ${{ secrets.CF_API_TOKEN }}
          ZONE_ID: ${{ secrets.CF_ZONE_ID }}

          REC_FUNCTION: ADD

          REC_TYPE: A
          REC_NAME: test.example.com
          REC_CONTENT: 1.2.3.4

          REC_TTL: 1
          REC_PROXIED: false


  - name: repository report
    uses: hani86400/actions/repository_report@main
    with:
      IGNORE_FILE: .gitignore
      TIME_ZONE: Asia/Riyadh
#     REPORT_SUBJECT: TEST_REPORT



  - name: send email
    uses: hani86400/actions/send_email@main
    with:
      API_URL: ${{ vars.RESEND_API_URL }}
      API_KEY: ${{ secrets.RESEND_API_KEY }}
      FROM:    ${{ env.EMAIL_FROM }}
      TO:      ${{ env.EMAIL_TO }}
      SUBJECT: ${{ env.REPORT_SUBJECT }}
      HTML:    ${{ env.REPORT_HTML }}   




```

---

# Repository Structure

```text
actions/
├── cloudflare/
├── hello/
├── repository_report/
└── send_email/
```
