# verdin-uptime

An uptime check for the Verdin production site, `https://verdinmapper.com/`.

## How it works

- The workflow `.github/workflows/uptime.yml` runs every 10 minutes on GitHub Actions. The repo is public, so the runs cost nothing.
- The site is "up" when it answers with HTTP 2xx or 3xx and the page contains the word `Verdin`. The check tries 3 times before it says "down".
- A run that finds the site down fails. The result of the previous run is the state, so the repo stores no other state.
- The workflow sends an email through Resend when the state changes: "DOWN" and "RECOVERED". While the site stays down, it sends a "STILL DOWN" reminder every 6 hours.

## Secrets

| Secret | Use |
|---|---|
| `RESEND_FACTORY_KEY` | The Resend key, Sending access on `mail.verdinmapper.com` |
| `FACTORY_MAIL_TO` | The address that gets the alarm |

Only a schedule or a manual start runs the workflow. A pull request never runs it, so a fork cannot read the secrets.

## Test the alarm

Start the workflow by hand with a bad URL in the `url` input. The run fails and sends "DOWN". The next scheduled run finds production up and sends "RECOVERED".

## Limit

GitHub turns off a schedule in a public repo after 60 days with no commit. A commit starts the 60 days again.

The design and the decisions are recorded in the `upn-mapper` issue #2446.
