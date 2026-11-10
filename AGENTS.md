# Repository Guidelines

## Project Structure & Module Organization

This repository contains Azure administration challenge labs, primarily as numbered Markdown files in the root. The sequence currently runs from `01-...md` through `21-...md`. Screenshots and other exercise images live in `img/`; reference them with relative paths such as `img/03-02.png`. `Readme.md` is the project overview. Files ending in ` - Copy.md` and similarly named image files are duplicate copies; make changes to the intended canonical exercise and avoid adding another duplicate unless explicitly needed.

## Build, Test, and Development Commands

There is no application build, package manager, or automated test suite. Edit the Markdown directly and preview it in a Markdown renderer. Check that image links and external references resolve, and review changes with `git diff --check` and `git diff` before committing.

## Content and Naming Conventions

Use the existing numbered filename pattern: two-digit exercise number, a descriptive title, and `.md` (for example, `12-Configure a Near Real-Time Metric Alert.md`). Keep each lab organized with descriptive Markdown headings, concise task instructions, tables for Azure resource settings, fenced blocks for commands or templates, and checklists for verification. Preserve Azure product and PowerShell cmdlet names accurately. Use relative paths for repository assets and retain the `img/` directory convention.

## Testing Guidelines

Since there are no automated tests, verify content manually: confirm steps are in a sensible order, resource names and values agree throughout the exercise, code fences are closed, and image paths match files in `img/`. For screenshot changes, ensure the referenced filename and extension match exactly.

## Commit & Pull Request Guidelines

Recent commits use short, lowercase summaries such as `adding 21st exercise` and `fix 18 with get-azvmextension`. Follow that concise style and state the exercise number or correction. Pull requests should summarize changed labs, explain the learner-facing impact, and include rendered screenshots when a visual change needs review. Link related issues when applicable.

## Security & Configuration Tips

These labs include example credentials and Azure resource names. Keep examples clearly scoped to training environments, never add real credentials, tokens, or subscription-specific secrets, and note when learners must substitute their own values.
