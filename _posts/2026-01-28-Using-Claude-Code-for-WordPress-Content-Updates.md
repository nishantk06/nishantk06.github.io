---
layout: post
categories: blog
title: Using Claude Code for WordPress Content Updates
published: true
---

I've been experimenting with Claude Code for the past few weeks, and today I stumbled onto a workflow that genuinely surprised me with how well it worked: using Claude Code to bulk-update WordPress content via the REST API.

If you manage a WordPress site and have ever needed to fix meta titles, update descriptions across dozens of pages, or create new content programmatically—this is for you.

## The Problem

I had a classic SEO housekeeping situation. My WordPress site had:

1. Missing or poorly written meta descriptions on several pages
2. Meta titles that weren't optimized for search
3. A need to create new landing pages quickly

Doing this manually through the WordPress admin? Tedious. Especially when you're dealing with 20+ pages.

## The Solution: Claude Code + WordPress REST API

Claude Code has access to bash commands and can make HTTP requests. WordPress has a REST API. Put them together, and you get a surprisingly powerful content management workflow.

Here's exactly how I did it today.

### Step 1: Set Up WordPress Application Password

First, you need authentication. WordPress supports Application Passwords (Settings → Users → Your Profile → Application Passwords).

I created one specifically for this task. You'll get a password that looks like `xxxx xxxx xxxx xxxx xxxx xxxx`. Keep it safe—you'll need it with your username for API calls.

### Step 2: List All Pages to See What Needs Fixing

I asked Claude Code to fetch all my pages so I could see the current state:

```bash
curl -s "https://yoursite.com/wp-json/wp/v2/pages?per_page=100" \
  -u "username:application-password" | jq '.[] | {id, title: .title.rendered, slug}'
```

This gave me a clean list of page IDs, titles, and slugs. From here, I could identify which pages needed attention.

### Step 3: Audit Meta Titles and Descriptions

For SEO metadata, I'm using Yoast SEO, which stores meta titles and descriptions in custom fields. Claude Code helped me pull the current values:

```bash
curl -s "https://yoursite.com/wp-json/wp/v2/pages/123" \
  -u "username:application-password" | jq '{
    title: .title.rendered,
    yoast_title: .yoast_head_json.title,
    yoast_description: .yoast_head_json.description
  }'
```

Running this across multiple pages quickly showed me which ones had weak or missing meta descriptions.

### Step 4: Update Meta Descriptions in Bulk

Here's where Claude Code really shined. I described what I wanted—better meta descriptions that included target keywords and stayed under 155 characters—and Claude generated the updates and executed them:

```bash
curl -X POST "https://yoursite.com/wp-json/wp/v2/pages/123" \
  -u "username:application-password" \
  -H "Content-Type: application/json" \
  -d '{
    "meta": {
      "_yoast_wpseo_metadesc": "Your optimized meta description here. Keep it compelling and under 155 characters."
    }
  }'
```

The key insight: Claude could read the existing page content, understand the context, and write meta descriptions that actually made sense—not generic filler text.

### Step 5: Create New Pages

Creating new pages was just as straightforward. I described what I needed, and Claude Code built and executed the API call:

```bash
curl -X POST "https://yoursite.com/wp-json/wp/v2/pages" \
  -u "username:application-password" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "New Landing Page Title",
    "content": "<!-- wp:paragraph --><p>Your page content here.</p><!-- /wp:paragraph -->",
    "status": "draft",
    "meta": {
      "_yoast_wpseo_title": "%%title%% | Your Site Name",
      "_yoast_wpseo_metadesc": "A compelling meta description for search results."
    }
  }'
```

Pages get created as drafts by default, so you can review them before publishing.

## What Made This Workflow Click

A few things made this genuinely useful rather than a gimmick:

**Context awareness.** Claude Code could read the existing page content before suggesting meta descriptions. This meant the descriptions were actually relevant, not just keyword-stuffed templates.

**Iteration speed.** When a meta description was too long or didn't quite hit the mark, I could say "make it shorter" or "focus more on the benefit" and get an updated version immediately.

**Error handling.** When an API call failed (wrong field name, authentication issue), Claude could read the error response and adjust the command.

**No context switching.** I stayed in the terminal the whole time. No clicking through WordPress admin pages, no copy-pasting between tools.

## Caveats

This isn't magic. A few things to keep in mind:

1. **You need the REST API enabled** (it is by default on most WordPress installs)
2. **Application Passwords require HTTPS** in production
3. **Custom fields vary by plugin**—Yoast uses different meta keys than RankMath or All in One SEO
4. **Always test on staging first** if you're doing bulk updates

## The Bigger Picture

What I'm realizing is that Claude Code isn't just for writing code. It's a capable automation layer for any system with an API. WordPress, Webflow, Shopify, Notion—if there's an API, you can probably manage it conversationally.

Today it was meta descriptions. Tomorrow it might be syncing content between systems or generating landing pages from a spreadsheet of keywords.

The terminal just got a lot more interesting.
