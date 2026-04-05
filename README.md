# n8n Workflow Backup

> 13 workflows (5 active, 6 inactive, 2 archived) — automated weekly backup via n8n

This repository is an automated backup of all n8n workflows running on a Hostinger VPS. Backups are committed every Friday at 6 PM (Europe/Dublin) by the [Weekly Workflow Backup to GitHub](#infrastructure) workflow. Each workflow lives in its own folder as `workflow.json`.

---

## Task & Productivity

Workflows that sync tasks between platforms, generate daily briefings, and recommend actions using AI.

| Status | Workflow | Folder | Nodes | Description |
|--------|----------|--------|-------|-------------|
| **Active** | GTN-A: Notion → Google Tasks Sync | [`gtn-a-notion-google-tasks-sync`](workflows/gtn-a-notion-google-tasks-sync) | 10 | One-way sync polling Notion every 10 min, pushes changes to Google Tasks with timestamp-based conflict resolution |
| **Active** | GTN-B: Google Tasks → Notion Sync | [`gtn-b-google-tasks-notion-sync`](workflows/gtn-b-google-tasks-notion-sync) | 9 | Reverse sync polling Google Tasks on staggered intervals, pushes changes to Notion |
| Inactive | GTN — Bidirectional Sync | [`gtn-notion-google-tasks-bidirectional-sync`](workflows/gtn-notion-google-tasks-bidirectional-sync) | 25 | Original monolithic bidirectional sync (superseded by the GTN-A + GTN-B pair) |
| **Active** | Daily Easy Task Recommender | [`daily-easy-task-recommender-for-notion`](workflows/daily-easy-task-recommender-for-notion) | 33 | Daily AI evaluation of task difficulty via Ollama, updates Notion properties, sends email summary via Gmail |
| Inactive | Morning Brief Advisor | [`morning-brief-advisor-final`](workflows/morning-brief-advisor-final) | 12 | Morning briefing aggregating Google Tasks + Notion data, AI prioritization via Ollama, delivered via Gmail |

---

## Content Processing

Automated content pipeline that processes YouTube videos and web articles saved to a Notion Links database.

| Status | Workflow | Folder | Nodes | Description |
|--------|----------|--------|-------|-------------|
| Inactive | Link Processor — Main | [`link-processor-main`](workflows/link-processor-main) | ~20 | Polls Notion for unprocessed links, classifies URLs (YouTube/Article), transcribes via Whisper (async), scrapes via Firecrawl, summarizes via Ollama, writes results back to Notion |
| Inactive | Link Processor — Retry | [`link-processor-retry`](workflows/link-processor-retry) | ~6 | Companion retry workflow (every 30 min) that resets Error/stale-Processing rows back to Queued, max 3 attempts |

---

## Price Monitoring

Scheduled scrapers that track listings, detect changes, and send alerts.

| Status | Workflow | Folder | Nodes | Description |
|--------|----------|--------|-------|-------------|
| Archived | Property Hunter | [`property-hunter`](workflows/property-hunter) | 17 | Scrapes real estate sites via Firecrawl, extracts structured listings via Ollama AI Agent, deduplicates against Supabase, sends Gmail alerts for new properties |

---

## Transcription

Audio/video transcription workflows using a local Whisper service.

| Status | Workflow | Folder | Nodes | Description |
|--------|----------|--------|-------|-------------|
| Inactive | Whisper Transcription Form | [`whisper-transcription-form`](workflows/whisper-transcription-form) | 7 | Web form accepting a YouTube URL or audio file upload, routes to local Whisper API, displays transcription result |

---

## Notion Utilities

Tools for inspecting, modifying, and seeding Notion databases.

| Status | Workflow | Folder | Nodes | Description |
|--------|----------|--------|-------|-------------|
| **Active** | Notion DB Schema Extractor | [`notion-db-schema-extractor`](workflows/notion-db-schema-extractor) | 7 | Web form that retrieves a Notion database schema via the API and renders it as formatted HTML |
| Inactive | Setup: Add AI Properties | [`setup-add-ai-properties-to-notion-db`](workflows/setup-add-ai-properties-to-notion-db) | 3 | One-time setup utility to add AI-related columns to a Notion database |
| Inactive | Seed Pluralsight Courses | [`seed-pluralsight-courses-to-notion`](workflows/seed-pluralsight-courses-to-notion) | 4 | One-time data import of Pluralsight course entries into a Notion database |

---

## Infrastructure

Workflows that maintain the n8n system itself.

| Status | Workflow | Folder | Nodes | Description |
|--------|----------|--------|-------|-------------|
| **Active** | Weekly Workflow Backup to GitHub | [`weekly-workflow-backup-to-github`](workflows/weekly-workflow-backup-to-github) | 10 | Runs Fridays at 6 PM, exports all non-archived workflows, sanitizes credentials, commits as atomic Git tree |

---

## Services Used

| Service | Role | Hosting |
|---------|------|---------|
| **n8n** | Workflow orchestrator | Hostinger VPS (Docker) |
| **Ollama** (qwen2.5) | AI summarization, task evaluation, data extraction | Hostinger VPS (Docker, internal) |
| **Whisper** | Audio/video transcription | Hostinger VPS (Docker, internal + public) |
| **Notion** | Task management, content database | Cloud API |
| **Google Tasks** | Task management | Cloud API |
| **Firecrawl** | Web scraping and content extraction | Cloud API |
| **Supabase** | Listing database for deduplication | Cloud |
| **Gmail** | Email notifications and briefings | Cloud API |
| **GitHub** | Workflow backup storage | Cloud API |
