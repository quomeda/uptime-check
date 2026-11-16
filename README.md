# Quomeda uptime check

Every 5 minutes, GitHub calls three public addresses:

| Address | Healthy when |
|---|---|
| `https://quomeda.com/api/health` | it answers 200: the database answers and every background job (campaign sending, mailbox import, transcripts, the morning emails, the nightly copies) got through recently |
| `https://quomeda.com/` | it answers 200: the website is served |
| `https://survey.quomeda.com/` | it answers 200: survey links work |

An address that fails is tried again 30 seconds later, so a single slow answer (during a publish, say) is not an alarm.

**When something is wrong**, the run fails and an issue titled **"Quomeda needs attention"** is opened, saying which address failed and, for the health page, which jobs. GitHub emails both. While it stays wrong, one reminder an hour. When it is fixed, the issue gets a "back up" comment and is closed, and that is emailed too.

This repository is public so its checks cost nothing: public repositories get unlimited free Actions minutes, while the private app repository's allowance is needed for its own CI. It holds the check and nothing else: no code, no data, no secrets. The issues record outages for anyone to see; they say which address failed and the job names, nothing more.

## Getting the emails

1. On this repository, **Watch** → **All Activity** (whoever created it already watches it).
2. github.com → your picture → **Settings** → **Notifications** → **System** → **Actions**:
   **Email**, and tick **Only notify for failed workflows**.
3. Make sure the email address in **Notifications** → **Default notifications email** is one that does not depend on quomeda.com: if the domain's mail breaks, the alert still has to arrive.

To test: **Actions** → **Quomeda uptime** → **Run workflow**. A healthy site gives a green run and no email.

## Good to know

- GitHub's timer is best-effort. Under load a 5-minute schedule can run late, so expect an alert 5 to 15 minutes after an outage starts.
- GitHub switches off schedules in a repository that has no commits for 60 days. The monthly **Keep the check on** workflow commits the date to `last-alive.txt` so that never happens.
- What to do when an alert comes in is in the app repository's `docs/backup-restore-and-uptime.md`, "When an alert comes in".
