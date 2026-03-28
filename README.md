# action_send_email

# Send Email via Resend API – GitHub Action
#
[![GitHub Action](https://img.shields.io/badge/action-send_email-blue?style=flat-square)](#)

A lightweight GitHub Action to send emails using the [Resend API](https://resend.com).  

Stop manually sending notifications, reports, or panic messages. Automate your emails straight from GitHub workflows, without cloning your entire repo every time.

---

## Features

- Send HTML emails easily
- Single recipient or multiple recipients
- Fully configurable via workflow inputs
- Can run as a step in any workflow
- No need to checkout your repo to send emails (finally)

---

## Usage

### Example Workflow

```yaml
name: TEST_SEND_EMAIL
on:
  workflow_dispatch:

jobs:
  send_email_job:
    runs-on: ubuntu-latest
    steps:
      - name: SEND_EMAIL
        uses: hani86400/action_send_email@v1
        with:
          API_URL: ${{ vars.RESEND_API_URL }}
          API_KEY: ${{ secrets.RESEND_API_KEY }}
          FROM: "from@example.com"
          TO: "to@example.com"
          SUBJECT: "Hello from GitHub Action"
          HTML: "<b>Automation continues its march</b>"


