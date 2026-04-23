# CodeCompanion: A Friendly Debugger

A minimal Next.js app that uses OpenAI gpt-3.5-turbo to review your code and flag bugs, security issues, and quality problems. Built as a college mini project.

## What it does

Paste any code into the editor, select the language, click **Analyse**. The AI reviews it across five categories — bugs, security, performance, code quality, and best practices — and returns structured findings with original code, a suggested fix, and a plain-language explanation.

## Setup

**1. Install dependencies**

```bash
npm install
```

**2. Add your OpenAI API key**

Create a file called `.env.local` in the project root:

```
OPENAI_API_KEY=sk-your-key-here
```

Get a key from https://platform.openai.com/api-keys. gpt-3.5-turbo costs roughly $0.001 per analysis, so a few hundred tests cost less than a dollar.

**3. Run the dev server**

```bash
npm run dev
```

Open http://localhost:3000.

## Project structure

```
src/
  lib/
    ai.ts                  ← All AI logic. One function: analyzeCode()
  app/
    page.tsx               ← The whole UI in one file
    layout.tsx             ← Fonts (Inter + JetBrains Mono) and metadata
    globals.css            ← Design tokens and base styles
    api/
      analyze/
        route.ts           ← POST /api/analyze — calls analyzeCode()
```

Four files. That's the entire application.

## How it works

`analyzeCode(code, language)` in `src/lib/ai.ts` sends a prompt to gpt-3.5-turbo asking it to return a JSON object with a summary and an array of issues. Each issue has a line number, severity, category, title, description, and a before/after code snippet. If the JSON parse fails, it falls back gracefully to showing the raw response rather than crashing.

The API route at `/api/analyze` is a thin wrapper — it validates the request body and calls `analyzeCode`, returning the result or an error message.

## What was removed from the original version

The original project had Google Gemini + OpenAI with a LangChain fallback system, four separate API routes (analyze, chat, and streaming versions of both), a chat assistant panel, a file uploader with ZIP and GitHub integration, and several hundred lines of component code across six component files.

All of that is gone. This version does one thing: you paste code, you get findings.

## Environment variables

| Variable         | Required | Description            |
|------------------|----------|------------------------|
| `OPENAI_API_KEY` | Yes      | Your OpenAI secret key |

The key is server-side only (no `NEXT_PUBLIC_` prefix), so it's never exposed to the browser.
