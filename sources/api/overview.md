page_title: Shippable API
page_description: How to interface with Shippable's API
page_keywords: shippable, API, HTTP

# API
# Overview and Purpose

The Shippable API enables you to do most things that you would normally do in the Shippable UI through HTTP requests.

Our API is RESTful, and can be interfaced through curl, third party tools, your own wrapper libraries, or any form of HTTPS communication.

Please note that you must have a paid account with Shippable in order to use the API.

## Endpoint

The main endpoint for interacting with our API is
<https://api.shippable.com>

---

## Authentication

Using our API requires authenticating with a Shippable API token. To generate a token, follow instructions in our [Adding an API token](/navigatingUI/accountSettings/api/) section.

These tokens must be placed in the header of your HTTP request. For example, if your API token has the value 10010, you can authenticate with curl as follows:

```bash
curl -H "Authorization: apiToken 10010" https://api.shippable.com
```

A useful pattern is to set an env var with the value of your token. For example, if we saved our token to the environment variable apiToken:

```bash
curl -H "Authorization: apiToken $apiToken" https://api.shippable.com
```

This is useful not only because one no longer has to type type apiToken in repeated times, but use of an env var allows for secure automation of API scripts; it is dangerous to directly save your apiToken into code.

Also be careful to never commit code containing your API token to a public repository. Doing so will compromise the security of your Shippable account. Treat your API token like a password.

Detailed documentation for API routes is provided below.

---
