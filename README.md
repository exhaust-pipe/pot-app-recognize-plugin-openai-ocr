# OpenAI OCR Plugin for Pot

[简体中文](README_zh.md) | English

An OCR (text recognition) plugin for [Pot](https://github.com/pot-app/pot-desktop) that uses an OpenAI-compatible Chat Completions API to extract text from images.

## Features

- Recognizes text in all languages supported by Pot.
- Accepts any vision-capable model name; the default is `gpt-4o`.
- Supports the official OpenAI API and OpenAI-compatible custom endpoints.
- Provides a customizable OCR prompt with a `$lang` placeholder.
- Sends images with high-detail processing and preserves reading order and line breaks by default.

## Installation

1. Download or clone this repository.
2. In Pot, open **Service Settings → Text Recognition**.
3. Add an external text-recognition plugin and select this packaged plugin file (download from release).
4. Enable **OpenAI OCR** and complete the configuration described below.

> The exact menu labels can vary between Pot versions. Refer to Pot's plugin management interface if your version uses different wording.

## Configuration

| Option | Required | Description |
| --- | --- | --- |
| **Model** | No | A vision-capable model exposed by your API provider. Defaults to `gpt-4o` when empty. |
| **Request Path** | No | API base URL or Chat Completions URL. Defaults to `https://api.openai.com`. |
| **API Key** | Provider-dependent | API key sent as a Bearer token. It is required by OpenAI and most compatible providers. |
| **Custom Prompt** | No | Instructions used for OCR. If empty, the built-in transcription prompt is used. `$lang` is replaced with the language selected in Pot. |

### Request path examples

The plugin normalizes the request path before sending a request:

- `https://api.openai.com` → `https://api.openai.com/v1/chat/completions`
- `api.example.com/v1` → `https://api.example.com/v1/chat/completions`
- `https://api.example.com/v1/chat/completions` → used unchanged

Your endpoint must accept OpenAI-compatible Chat Completions requests with image input and return the recognized text in `choices[0].message.content`.

## Custom prompt

Set **Custom Prompt** when you need output tailored to a particular document. You can include `$lang`, which the plugin replaces with Pot's selected language code. For example:

```text
Transcribe all visible text in reading order. The expected language is $lang. Output only the recognized text.
```

When the field is empty, the plugin asks the model to reproduce all visible text exactly, retain line breaks and reading order, and avoid translation or commentary.

## Usage

1. Select **OpenAI OCR** as the text-recognition service in Pot.
2. Capture a screen region or provide an image through Pot's normal OCR workflow.
3. Pot displays the text returned by the configured model.

## Privacy and security

Images are sent to the API endpoint configured in **Request Path**. Review your provider's privacy and data-retention policies before processing sensitive material. Keep API keys private and never commit them to this repository.

## Troubleshooting

- **Authentication errors:** Verify the API key and confirm that it can access the selected model.
- **Model errors:** Use a model that supports image input through the Chat Completions API.
- **404 or endpoint errors:** Check the request path. Supplying a base URL is usually sufficient because the plugin appends `/v1/chat/completions`.
- **Unexpected output:** Clear the custom prompt to restore the default, or explicitly request text-only output.

## License

This project is distributed under the [GNU License](LICENSE).
