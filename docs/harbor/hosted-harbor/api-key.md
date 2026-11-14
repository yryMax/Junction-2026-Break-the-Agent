> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Harbor Hub API key

> Create a Harbor Hub API key

To authenticate any command via the [CLI](https://docs.harborframework.com/hosted-harbor/cli.md) or
[API](https://docs.harborframework.com/hosted-harbor/api.md), you must first create a Harbor API key.

## CLI

If using the CLI, an API key is minted automatically when you successfully run `harbor auth login`.
The key is stored in `~/.harbor/credentials.json` and revoked upon `harbor auth logout`.
You can also authenticate the CLI via the `HARBOR_API_KEY` environment variable. The next
section demonstrates creating an API key via the web UI.

## Web UI

To mint an API key on the web UI, first go to [the Hub](https://hub.harborframework.com).

Click on your profile in the top-right corner:

![The Harbor Hub home page, listing published datasets](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/hub-home.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=f86605d6cc0529b7a5f16d339a442abb)

Then click the **Settings** tab:

![Profile page](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/hub-settings.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=5feff5526a2797eeb0bc015a7e7d1b31)

Scroll down to the API keys section and click **Create API key**. Give the key a name
and an expiration date. Store this key somewhere safe. It has full access to your Harbor Hub account.

![Profile page](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/harbor-api-key.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=4a72af19e81f2d2b5fac83ac892bd567)


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.