# Markup-Translator-Google-Gemini
## Copyright Usage and License
Please read [MIT license](LICENSE.md) for details on the license and what you can do with this code.
Other Links:  
- [GitHub Copyright and Licenses](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository)
- [GitHub Terms of Service](https://docs.github.com/en/site-policy/github-terms/github-terms-of-service)

# Quick Information
Markup Translator with Google Gemini AI teaches you how to create an automated README.md translator using only a Free Google Gemini API key (Google account) and your GitHub Account. Tutorial to make a JavaScript and .github workflow with a YAML file for Actions to automate Free translations for README.md files without any third-party GitHub Community Repository Tools, except Google.

# 🌍 Automated README Translation with Google Gemini AI
This MIT open-source tutorial teaches you how to include or add the **Google Gemini API** through Google AI Studio to translate `README.md` into multiple languages automatically using GitHub Actions.
The translation system was designed to reduce API usage, avoid unnecessary translation requests, and keep all translated README files organized in a dedicated `languages/` directory.
Instead of translating the README every time you push code to GitHub, this workflow runs manually whenever you trigger it through GitHub Actions.

## ✨ Features

- 🤖 Powered by Google Gemini AI.
- 🆓 Uses the Gemini API free tier, subject to Google's model availability and usage limits.
- 🌐 Supports multiple languages.
- ⚡ Translates three languages using a single Gemini API request.
- 🎯 Manual execution through GitHub Actions.
- 🔄 Automatic retry for temporary API errors.
- 📁 Stores translations in a dedicated `languages/` directory.
- 💾 Automatically commits and pushes translated files to GitHub.
- 📝 Preserves Markdown formatting, code blocks, links, images, and tables.

## 🛠️ Technologies Used

| Technology | Purpose |
| --- | --- |
| Google AI Studio | Creating and managing Gemini API keys |
| Gemini API | AI-powered README translation |
| Gemini Flash-Lite | Lightweight translation model |
| Node.js 24 | Running the translation script |
| GitHub Actions | Automating the translation workflow |
| GitHub Secrets | Securely storing the API key |

## 🌎 Supported Languages

The current translation configuration supports any language, as it uses an AI model. Examples of the samples I used below:

| Language | Code | Output file |
| --- | --- | --- |
| French | `fr` | `languages/README.fr.md` |
| Russian | `ru` | `languages/README.ru.md` |
| Spanish | `es` | `languages/README.es.md` |

- Additional languages can be added by modifying the language configuration in the translation script.

## 🌍 README Translation Policy
README translations are updated **only when necessary**, to conserve the Google Gemini API free-tier usage and respect its rate limits.

You can manually trigger the translation workflow only when the original `README.md` undergoes substantial changes, such as:

- Major content updates or restructuring.
- Significant new sections or features.
- New information, evidence, or documentation.
- Substantial additions that change the meaning or context of the README.

Minor spelling mistakes, grammar corrections, formatting adjustments, and small wording changes do not justify regenerating translations.

**The goal is to keep translations accurate and up to date without wasting API requests on insignificant changes.**

# 🚀 Setup Guide

## 1\. Create a Gemini API Key Using Google AI Studio

Google AI Studio provides access to the Gemini API and allows developers to create API keys for their applications.

**Steps:**

1. Visit [Google AI Studio](<https://aistudio.google.com/>).
2. Sign in using your Google account.
3. Open the [API Keys page](<https://aistudio.google.com/app/apikey>).
4. Click **Create API Key**.
5. Select an existing Google Cloud project or create a new project.
6. Copy your generated Gemini API key.

Google AI Studio automatically creates a project and API key for new users when appropriate.

**Important:** The Gemini API has a free tier with limited model availability, request limits, and token limits. Check Google's current pricing and usage restrictions before using it.

Useful links:

- [Google AI Studio](<https://aistudio.google.com/>)
- [Google AI Studio API Keys](<https://aistudio.google.com/app/apikey>)
- [Gemini API Documentation](<https://ai.google.dev/gemini-api/docs>)
- [Gemini API Pricing](<https://ai.google.dev/gemini-api/docs/pricing>)
- [Gemini API Rate Limits](<https://ai.google.dev/gemini-api/docs/rate-limits>)

## 2\. Configure GitHub Secrets

The Gemini API key must be stored securely in GitHub Secrets.

Never directly insert your API key into your JavaScript files or GitHub Actions workflow.

**Steps:**

1. Open your GitHub repository.
2. Navigate to **Settings**.
3. Select **Secrets and variables → Actions**.
4. Click **New repository secret**.
5. Configure the secret as follows:

| Field | Value |
| --- | --- |
| Name | `GEMINI_API_KEY` |
| Secret | Your Google AI Studio API key |

6. Click **Add secret**.

GitHub Actions can now securely access the API key during workflow execution.

## 3\. Create the Translation Script

Create the following file:

`scripts/translate-readme.mjs`

The actual path should be `scripts/translate-readme.mjs`.

This script handles the translation process.

It performs the following operations:

1. Reads the original `README.md`.
2. Sends the README content to Gemini.
3. Requests [Your Languages Here]. My example: French, Russian, and Spanish translations in one API request.
4. Requests structured JSON output.
5. Validates that all translations in 1 or more languages are present.
6. Saves each translation into its corresponding Markdown file.

The script also retries automatically for temporary API errors, such as HTTP 503 (Service Unavailable), HTTP 429 (Too Many Requests), and other temporary server errors.

This helps prevent temporary Gemini API problems from immediately failing the entire workflow.

### Why use one API request?

Instead of sending [number of languages selected, value, ex: 3 for mine] separate requests for French, Russian, and Spanish, the script asks Gemini to translate all three languages in one request.

This reduces the number of API requests per manual translation run from three to one.

**Note:** This reduces request count, not necessarily token consumption. The combined response still needs enough output tokens to contain all three translations.

## 4\. Configure the Gemini Model

The translation script uses the following model configuration:

`gemini-3.5-flash-lite`

We selected this model for lightweight text generation suitable for README translation.

The model name is configured in:

`const MODEL = "gemini-3.5-flash-lite";`

Model availability and free-tier eligibility can change. Check the [official Gemini models documentation](<https://ai.google.dev/gemini-api/docs/models>) if the API returns a model availability error.

## 5\. Create the GitHub Actions Workflow

Create the following file:

`.github/workflows/translate.yml`

The workflow uses:

- `workflow_dispatch` to enable manual execution.
- Use the new version of **Node.js 24** to run the translation script, as Node 12 and 20 are deprecated and will be removed in 2026.
- Use GitHub Secrets to access the Gemini API key.
- Use Git to commit and push the generated translations.

The workflow does not automatically execute when pushing code to `main` or `master`.

This is intentional if you want to control when Gemini API requests are made and avoid unnecessary free-tier usage.

## 6\. Run the Translation Manually

Once everything is configured, follow these steps:

1. Open your GitHub repository.
2. Navigate to the **Actions** tab.
3. Select **Translate README with Gemini**.
4. Click **Run workflow**.
5. Select the branch you want to use.
6. Click the green **Run workflow** button.

GitHub Actions will then:

1. Check out the repository.
2. Set up Node.js.
3. Read the original `README.md`.
4. Send one translation request to Gemini.
5. Receive [Your Languages], e.g., French, Russian, and Spanish translations.
6. Validate the returned translations.
7. Save the three translated Markdown files.
8. Commit and push the changes to the repository.

The workflow only runs when manually triggered.

---

# Results on GitHub Actions SAMPLE
For now, my sample shows X RED meaning error, but it is because I did not add the GITHUB SECRETS API KEY! Once you change the key to yours, the ACTION should work and turn CHECKMARK YES GREEN.
### On GitHub Actions, this tutorial page shows this error, so ignore it, as it's a demo and should be green.
```console
Run mkdir -p languages
  mkdir -p languages
  shell: /usr/bin/bash -e {0}
0s
Run node scripts/translate-readme.mjs
file:///home/runner/work/Markup-Translator-Google-Gemini/Markup-Translator-Google-Gemini/scripts/translate-readme.mjs:14
  throw new Error("GEMINI_API_KEY is missing.");
        ^
REPLACE IT WITH YOUR API KEY!!!!!!
Error: GEMINI_API_KEY is missing.
    at file:///home/runner/work/Markup-Translator-Google-Gemini/Markup-Translator-Google-Gemini/scripts/translate-readme.mjs:14:9
    at ModuleJob.run (node:internal/modules/esm/module_job:561:25)
    at async node:internal/modules/esm/loader:647:26
    at async asyncRunEntryPointWithESMLoader (node:internal/modules/run_main:101:5)

Node.js v24.22.2
Error: Process completed with exit code 1.
```

# 📁 Project Structure Example

The resulting project structure looks like this with my current sample:

```text
Main Branch Root/
│
├── .gith(─ wor)kflowsthat
│       └── translate.yn't
├── scripts/
│   └── translatkeyeadme.mjs
│
├── README.md
│
└── languages/
    ├── README.fr.md
    ├── README.ru.md
    └── README.es.md

- (Changes if you choose different languages)

```

### File descriptions Example

| File | Description |
| --- | --- |
| `README.md` | Original English README |
| `scripts/translate-readme.mjs` | Gemini translation script |
| `.github/workflows/translate.yml` | Manual GitHub Actions workflow |
| `languages/README.fr.md` | French translation |
| `languages/README.ru.md` | Russian translation |
| `languages/README.es.md` | Spanish translation |

---

# 💡 Why Choose This Method: API KEYS

Using old Community Tools from other repositories with  GitHub Actions to generate README translations is outdated and deprecated with the new Node.js v24 and the removal of v20
The benefits include gaining control over the translation process, API usage, and the output directory.
Instead of relying on an external translation action from GitHub Tool Repositories (Communities), you can build a small custom translation script using Google's Gemini API.

### Main reasons for this approach

**1\. Free-tier API usage**

Google AI Studio lets me access eligible Gemini models through the free API tier, subject to Google's usage limits.

**2\. Fewer API requests**

The original approach required three separate API requests for three languages.
The new implementation combines all three translations into one request, reducing the number of requests.

**3\. Manual control**

The goal is not to allow Gemini to translate my README every time you push a code change.
This lets you manually trigger the translation workflow only when you update your README and want to regenerate its translations.

**4\. Better error handling**

The custom JavaScript script includes automatic retries for temporary API failures.

It also checks that Gemini returned all three translations before writing the output files.

**5\. Organized file management**

All translations are stored in one directory, keeping the repository root clean.

**6\. No external translation action dependency**

The translation process communicates directly with Google's Gemini API instead of relying on a third-party GitHub translation action, Free Tools in the Community.

---

# 📊 API Usage Optimization Example and Expected Results

The main objective is to minimize unnecessary Gemini API requests.

| Translation method | Requests per run |
| --- | --- |
| Three separate translation requests | 3 |
| One combined translation request | 1 |

The combined method reduces the request count by approximately 67%.
For example, if you manually run the translation workflow 10 times:

| Method | Total requests |
| --- | --- |
| Three separate requests | 30 |
| One combined request | 10 |

Actual token consumption depends on README length and translation output.

You can monitor your usage through:

[Google AI Studio Dashboard](<https://aistudio.google.com/>)

[Gemini API Rate Limits](<https://ai.google.dev/gemini-api/docs/rate-limits>)

[Gemini API Pricing](<https://ai.google.dev/gemini-api/docs/pricing>)

---

# ⚠️ WARNINGS: Important Notes With API KEYS and Free-Tier Limitations

- Never publish your Gemini API key.
- Always store the API key in GitHub Secrets.
- Free-tier limits depend on the selected model and Google's current policies.
- A free-tier API key does not guarantee unlimited requests.
- HTTP 503 errors can occur when Gemini experiences high demand.
- Automatic retries help with temporary failures but cannot guarantee availability.
- Large README files may exceed the model's output token limit.
- Always review generated translations for accuracy.
- The free-tier service has its own data-use terms; review Google's policies before sending sensitive content.

---

# 🔗 Useful Resources

| Resource | Link |
| --- | --- |
| Google AI Studio | [https://aistudio.google.com/](<https://aistudio.google.com/>) |
| Gemini API Keys | [https://aistudio.google.com/app/apikey](<https://aistudio.google.com/app/apikey>) |
| Gemini API Documentation | [https://ai.google.dev/gemini-api/docs](<https://ai.google.dev/gemini-api/docs>) |
| Gemini Models | [https://ai.google.dev/gemini-api/docs/models](<https://ai.google.dev/gemini-api/docs/models>) |
| Gemini API Pricing | [https://ai.google.dev/gemini-api/docs/pricing](<https://ai.google.dev/gemini-api/docs/pricing>) |
| Gemini Rate Limits | [https://ai.google.dev/gemini-api/docs/rate-limits](<https://ai.google.dev/gemini-api/docs/rate-limits>) |
| GitHub Actions | [https://docs.github.com/en/actions](<https://docs.github.com/en/actions>) |
| GitHub Secrets | [https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions](<https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions>) |

---

**Final Result:** Yay! You have completed the tutorial! A manually controlled README translation system powered by Google Gemini AI, using one API request to generate language translations, with automatic retries, organized output files, and automatic GitHub commits.

## A few important notes about your implementation

1. Free tier: Google's free tier has model-specific limits and availability. Google also states that it may use free-tier content to improve its products, so avoid sending confidential material.
   Google AI for Developers
   +1
2. Model: Your `gemini-3.5-flash-lite` model should be described as the model currently configured in your script, not a permanently guaranteed free model.
3. One request: This means one HTTP API call for all three translations, not one request per language. A large README may require a different strategy if the combined output exceeds the model's limits.

Useful official references:

- [Create a Gemini API key](https://aistudio.google.com/app/apikey)
- [Gemini API free-tier pricing](https://ai.google.dev/gemini-api/docs/pricing)
- [Gemini API usage limits](https://ai.google.dev/gemini-api/docs/rate-limits)
- [GitHub Actions documentation](https://docs.github.com/en/actions)
- **NOTE: THIS TUTORIAL WAS BUILT WITH BOTH AI INTEGRATION (CHATGPT) AND MANUAL USER MODIFICATIONS TO MAKE IT READABLE!!! IT'S WHY THIS PUBLIC COMMUNITY TOOL REPOSITORY IS USING THE MIT LICENSE AND IS ALSO OPEN SOURCE (NO CREDIT TO ME, NO CHATGPT, AND FOR EDUCATIONAL USAGES!!!)**

