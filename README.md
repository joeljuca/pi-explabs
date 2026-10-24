# @joeljuca/pi-explabs

[![npm version](https://img.shields.io/npm/v/@joeljuca/pi-explabs.svg?color=red&logo=npm)](https://www.npmjs.com/package/@joeljuca/pi-explabs)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg?logo=apache)](LICENSE)
[![GitHub](https://img.shields.io/badge/joeljuca/pi--explabs-white?logo=github&logoColor=black)](https://github.com/joeljuca/pi-explabs)
[![Codeberg](https://img.shields.io/badge/joeljuca/pi--explabs-blue?logo=codeberg&logoColor=white)](https://codeberg.org/joeljuca/pi-explabs)
[![Author](https://img.shields.io/badge/joeljuca.com-white?logo=google-chrome)](https://joeljuca.com)

[Experiential Labs](https://experientiallabs.ai) provider extension for the [pi coding agent](https://pi.dev), featuring **dynamic model discovery** — models are fetched from the Experiential Labs API whenever pi refreshes the model list.

## Features

- 🔍 **Dynamic model discovery** — models are fetched live from the Experiential Labs API
- 🔄 **Auto-refresh** — model list updates when you open `/model` in pi
- 🛠️ **Zero configuration** — just set your API key and go
- 🔑 **Flexible credentials** — read your key from pi's `auth.json`, or set an environment variable
- 🔌 **Multiple API formats** — supports Chat Completions, Responses, and Anthropic Messages APIs

## Set up

First, you'll obviously need pi installed in your system. Instructions at [pi.dev](https://pi.dev).

Then, installing the extension is trivial:

```
pi install npm:@joeljuca/pi-explabs
```

You'll need an API key to authenticate. Get one at [experientiallabs.ai](https://experientiallabs.ai).

### Option 1: pi's credential store (recommended)

Add an `explabs` entry to your `auth.json` (usually `~/.pi/agent/auth.json`). If you keep secrets in a password manager, point `key` at a command prefixed with `!` and the key is resolved on demand instead of being written to disk in plain text:

```json
{
  "explabs": {
    "type": "api_key",
    "key": "<Your API key here>"
  }
}
```

```json
{
  "explabs": {
    "type": "api_key",
    "key": "!secret-tool lookup service explabs"
  }
}
```

Values may also reference an environment variable, like `$EXPLABS_API_KEY`.

### Option 2: environment variable (fallback)

When `auth.json` has no `explabs` entry, pi falls back to the `EXPLABS_API_KEY` environment variable. I use and recommend [direnv](https://direnv.net), but any method will do.

```sh
EXPLABS_API_KEY='<Your API key here>'
```

`auth.json` wins when both are set: a stored credential always takes priority, and the environment variable is only consulted when nothing is stored — the same rule pi applies to every provider.

You're ready. Run pi normally, the extension should be automatically:

```
pi
```

## Use

Usage is mostly automatic. The extension will make your pi integrate seamlessly with Experiential Labs, so you should be able to see their models when you `/model`, the list will be reloaded when you `/reload`, etc.

Here's what the extenstion will do:

1. It registers `explabs` as a model provider in pi
1. It resolves your API key through pi's credential store: the `explabs` entry in `auth.json` first, `EXPLABS_API_KEY` as the fallback
1. Fetches a fresh list of available models from Experiential Labs when you open pi
1. Refresh the list of models when you `/model` or `/reload`
1. **API fetch**: The callback fetches models from the Experiential Labs `/v1/models` endpoint

> 💡 Troubleshoot
>
> If something is not working properly, you're not seeing Experiential Labs' models when you `/model`, etc., the first place to check is your API key. Run pi with `DEBUG=pi-explabs` to see which key source the extension registered (`config` or `env:$EXPLABS_API_KEY`) and which credential pi resolved for the model refresh — the key itself is never logged.
>
> If you're using `auth.json`, check the entry:
>
> ```sh
> cat ~/.pi/agent/auth.json
> ```
>
> If you're relying on the environment variable:
>
> ```sh
> echo $EXPLABS_API_KEY
> ```
>
> Test it against the API:
>
> ```bash
> curl https://api.experientiallabs.ai/v1/models -H "Authorization: Bearer $EXPLABS_API_KEY"
> ```
>
> Reload pi (while in pi, run `/reload`). Check pi's console output for error messages, you'll most certainly find insights on what's wrong.
>
> If nothing works and you believe it's a bug in the extension itself, open an issue at [https://github.com/joeljuca/pi-explabs/issues](https://github.com/joeljuca/pi-explabs/issues).

## Contribute

See: [CONTRIBUTING.md](CONTRIBUTING.md)

## License

[Apache-2.0](LICENSE) © [Joel Jucá](https://joeljuca.com)
