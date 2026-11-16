# Instant Link Postman

Sandbox collection for Plaid Check Instant Link. The requests live in Postman Collection v3 YAML so Postman Native Git can open this repository directly.

Instant Link does not open Link. Create a user, then call `POST /cra/report/get` with `scope: PLAID_NETWORK` and no `report_id`.

## Use it in the Postman web app

The web app cannot attach to a GitHub repo. Postman documents the Git-connected experience as desktop-only. The web app can import a collection file.

1. On GitHub, open https://github.com/wwilliams96/instant-link-postman and download these two files (or use the copies in this folder):
   - `instant-link.postman_collection.json`
   - `instant-link-sandbox.postman_environment.json`
2. In [Postman on the web](https://go.postman.co), click **Import**.
3. Upload both files.
4. Open the **Instant Link** collection, select the **Instant Link - Sandbox** environment, and set `client_id` and `secret`.

That import is a copy. Later edits in the web app do not write back to GitHub.

## Connect the repo in Postman desktop

The remote is a private repo on github.com: `wwilliams96/instant-link-postman`. The desktop app reads the YAML under `postman/` from this checkout.

1. Install [Postman desktop](https://www.postman.com/downloads/) (v12). Git needs to be on your PATH; Postman uses that Git, not its own.
2. In Postman, open **Settings → Connected accounts** (or the GitHub prompt the first time you connect a repo) and authorize **github.com** for the `wwilliams96` account.
3. In the local filesystem sidebar, choose **Open folders** and select this repository root (`instant-link-postman`).
4. Connect the folder to a workspace when Postman asks. `.postman/resources.yaml` is the workspace binding after that.
5. Open the **Instant Link** collection and select the **Instant Link - Sandbox** environment.

On another machine: `git clone git@github.com:wwilliams96/instant-link-postman.git`, then open that folder in Postman.

## Credentials

The Sandbox key must have Consumer Reports enabled.

In the environment, set `client_id` and `secret` from the Plaid Dashboard Sandbox keys. Leave those values out of git. If Postman writes them back into `postman/environments/instant-link-sandbox.environment.yaml`, do not commit that file.

`sandbox_phone` defaults to `+14155550011`, the Sandbox number used for a successful network match.

## Run

Send the requests in order:

1. **Create user** — writes `user_id` onto the collection and the environment.
2. **Update user identity** — optional. The saved body sets the same sandbox phone, so a full collection run still matches. Change the phone when you are retrying a weak match.
3. **Get Instant Link report** — expects `report.scope` of `PLAID_NETWORK` and no Base Report.

`NO_MATCHING_USER` means that identity did not match the network. This collection does not fall back to Standard Link.

## Cloud sync

Local View does not upload to Postman Cloud on its own. To push later, install the [Postman CLI](https://learning.postman.com/docs/postman-cli/postman-cli-installation/), put your workspace id in `.postman/resources.yaml`, and run `postman workspace push` from this directory.
