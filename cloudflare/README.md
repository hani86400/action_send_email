# LIB_CLOUDFLARE

GitHub Composite Action to manage CloudFlare DNS records using the CloudFlare API.

Features:

* Add DNS records
* Delete DNS records
* List DNS records
* Generate HTML DNS report
* Time zone support
* Supports proxied/non-proxied records
* Supports configurable TTL

---

# Inputs

| Input          | Required | Default | Description                                       |
| -------------- | -------- | ------- | ------------------------------------------------- |
| `API_TOKEN`    | YES      | -       | CloudFlare API Token                              |
| `ZONE_ID`      | YES      | -       | CloudFlare Zone ID                                |
| `REC_FUNCTION` | YES      | `LIST`  | Function: `ADD`, `DEL`, `LIST`                    |
| `REC_TYPE`     | NO       | `NONE`  | DNS record type (`A`, `TXT`, `CNAME`, `MX`, etc.) |
| `REC_NAME`     | NO       | `NONE`  | DNS record name                                   |
| `REC_CONTENT`  | NO       | `NONE`  | DNS record content                                |
| `REC_TTL`      | NO       | `1`     | DNS record TTL                                    |
| `REC_PROXIED`  | NO       | `false` | Enable CloudFlare proxy                           |
| `TIME_ZONE`    | NO       | `UTC`   | Linux time zone                                   |

---

# Example: LIST Records

```yaml
name: TEST_CLOUDFLARE_LIST

on:
  workflow_dispatch:

jobs:

  TEST:
    runs-on: ubuntu-latest

    steps:

      - uses: actions/checkout@v4

      - name: LIST_DNS_RECORDS
        uses: hani86400/actions/cloudflare@main
        with:
          API_TOKEN: ${{ secrets.CF_API_TOKEN }}
          ZONE_ID: ${{ secrets.CF_ZONE_ID }}
          REC_FUNCTION: LIST
```

---

# Example: ADD Record

```yaml
name: TEST_CLOUDFLARE_ADD

on:
  workflow_dispatch:

jobs:

  TEST:
    runs-on: ubuntu-latest

    steps:

      - uses: actions/checkout@v4

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
```

---

# Example: DELETE Record

```yaml
name: TEST_CLOUDFLARE_DELETE

on:
  workflow_dispatch:

jobs:

  TEST:
    runs-on: ubuntu-latest

    steps:

      - uses: actions/checkout@v4

      - name: DELETE_RECORD
        uses: hani86400/actions/cloudflare@main
        with:
          API_TOKEN: ${{ secrets.CF_API_TOKEN }}
          ZONE_ID: ${{ secrets.CF_ZONE_ID }}

          REC_FUNCTION: DEL

          REC_TYPE: A
          REC_NAME: test.example.com
          REC_CONTENT: 1.2.3.4
```

---

# Example: ADD Then REPLACE Record

The action automatically deletes matching records before adding new ones when using:

```yaml
REC_FUNCTION: ADD
```

This helps prevent duplicate DNS records.

Because duplicate DNS entries are one of humanity’s favorite hobbies right beside unread documentation and rebooting production servers “just to test something.”

---

# HTML Report

The action generates an HTML report:

```text
/tmp/cf_report
```

The report includes:

* ID
* TYPE
* NAME
* CONTENT
* PROXIED
* TTL
* CREATED_ON
* MODIFIED_ON

---

# Required CloudFlare API Permissions

Recommended API token permissions:

| Permission | Access |
| ---------- | ------ |
| Zone.Zone  | Read   |
| Zone.DNS   | Edit   |

---

# Example API Token Scope

Recommended scope:

```text
Zone -> DNS -> Edit
Zone -> Zone -> Read
```

Restrict the token to specific zones whenever possible.

Because giving global DNS write access to automation is how future-you creates exciting incident reports.

---

# Notes

* `REC_FUNCTION` is automatically converted to uppercase.
* `ADD` automatically attempts cleanup of matching records before insert.
* HTML output is printed to workflow logs.
* Uses CloudFlare v4 API.

---

# Dependencies

Ubuntu runner packages:

* `bash`
* `curl`
* `jq`

GitHub Ubuntu runners already include them.

For now. The industry enjoys randomly removing useful tools every few years in the name of “modernization.”
