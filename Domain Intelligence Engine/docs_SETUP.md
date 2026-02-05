# 🚀 Setup Guide

> Step-by-step instructions to get the workflow running in under 30 minutes

## Prerequisites

Before starting, ensure you have:

- [ ] n8n instance (self-hosted or cloud)
- [ ] Google account (for Google Sheets)
- [ ] Exa.ai account (free tier available)
- [ ] OpenAI account (API access)
- [ ] Telegram account (optional, for notifications)

---

## Step 1: Create Google Sheet (5 min)

### Option A: Use Template

1. Download `templates/google-sheets-template.xlsx` from this repo
2. Go to [Google Sheets](https://sheets.google.com)
3. File → Import → Upload the template
4. Note the **Document ID** from the URL:
   ```
   https://docs.google.com/spreadsheets/d/[THIS-IS-YOUR-DOCUMENT-ID]/edit
   ```

### Option B: Create Manually

1. Create a new Google Sheet
2. Name it "AI Infrastructure Learning" (or your preference)

3. **Create Tab 1: "Keywords"**
   
   Add these headers in Row 1:
   | A | B | C | D | E |
   |---|---|---|---|---|
   | Keyword | Industry | Sub_Industry | Priority | Status (Active/Inactive) |

4. **Create Tab 2: "Sheet4"**
   
   Add these headers in Row 1:
   | A | B | C | D | E | F | G | H |
   |---|---|---|---|---|---|---|---|
   | Date (datetime) | Title | Category (FOUNDATIONAL/INTERMEDIATE/ADVANCED/RESEARCH) | Difficulty (1-10) | Quality_Score (1-10) | Learning Summary | Prerequisites | URL |

5. **Add sample keywords** to the Keywords tab:

   | Keyword | Industry | Sub_Industry | Priority | Status (Active/Inactive) |
   |---------|----------|--------------|----------|--------------------------|
   | model serving | AI Infrastructure | Model Serving | High | Active |
   | vLLM | AI Infrastructure | Model Serving | High | Active |
   | inference cost | AI Infrastructure | Cost Optimization | High | Active |

---

## Step 2: Get API Keys (10 min)

### Exa.ai API Key

1. Go to [exa.ai](https://exa.ai)
2. Click "Get Started" or "Sign Up"
3. Complete registration
4. Navigate to Dashboard → API Keys
5. Create new API key
6. **Copy and save the key** (starts with something like `5f07...`)

**Free tier includes:** 1,000 searches/month (plenty for this workflow)

### OpenAI API Key

1. Go to [platform.openai.com](https://platform.openai.com)
2. Sign up or log in
3. Click your profile → "View API Keys"
4. Click "Create new secret key"
5. Name it "n8n-learning-workflow"
6. **Copy and save the key** (starts with `sk-...`)

**Cost estimate:** ~$1-2/month for this workflow

### Telegram Bot (Optional)

1. Open Telegram and search for `@BotFather`
2. Send `/newbot`
3. Follow prompts to name your bot
4. **Copy the bot token** (format: `123456789:ABCdefGHI...`)
5. Search for `@userinfobot` and send any message
6. **Copy your Chat ID** (a number like `7991295449`)

---

## Step 3: Import Workflow to n8n (5 min)

### For n8n Cloud

1. Log in to your n8n cloud instance
2. Go to Workflows → Add Workflow → Import from File
3. Select `workflows/AI_Infrastructure_Learning_v7.json`
4. Click Import

### For Self-Hosted n8n

1. Open your n8n instance
2. Click the hamburger menu (☰) → Import from File
3. Select the JSON file
4. Click Import

---

## Step 4: Configure Credentials (10 min)

### Google Sheets OAuth2

1. In n8n, go to **Settings → Credentials**
2. Click **Add Credential → Google Sheets OAuth2**
3. Follow the OAuth flow to connect your Google account
4. Name it "Google Sheets account"

### Update Google Sheet Node

1. Open the imported workflow
2. Click on **"Read Keywords"** node
3. Under "Document", click the dropdown and select your sheet
4. Under "Sheet", select "Keywords"
5. Repeat for **"Save to Sheet4"** node:
   - Select same document
   - Select "Sheet4" tab

### Exa.ai API Key

1. Click on **"Exa Neural Search"** node
2. Find the Headers section
3. Update `x-api-key` value with your Exa API key

### OpenAI Credential

1. Go to **Settings → Credentials**
2. Click **Add Credential → OpenAI**
3. Paste your OpenAI API key
4. Name it "OpenAi account"
5. In workflow, click **"GPT Write Summaries"** node
6. Select your OpenAI credential

### Telegram Credential (Optional)

1. Go to **Settings → Credentials**
2. Click **Add Credential → Telegram**
3. Paste your Bot Token
4. Name it "Telegram Bot"
5. In workflow, click **"Send Telegram"** node
6. Select your Telegram credential
7. Update **Chat ID** field with your chat ID

---

## Step 5: Test the Workflow (5 min)

### Manual Execution

1. Click **"Execute Workflow"** button
2. Watch the execution progress through each node
3. Check for any red error indicators

### Verify Outputs

**Google Sheets:**
- Open your Google Sheet
- Check "Sheet4" tab for new rows
- Verify all columns are populated

**Telegram (if configured):**
- Check your Telegram for the digest message
- Verify formatting looks correct

### Common First-Run Issues

| Issue | Solution |
|-------|----------|
| "No active keywords found" | Add keywords to Keywords tab with Status="Active" AND Priority="High" |
| Exa returns empty | Check API key is correct; try broader keywords |
| Google Sheets permission denied | Re-authenticate OAuth; check sheet sharing settings |
| GPT timeout | Check OpenAI API key; verify account has credits |

---

## Step 6: Activate Automation (1 min)

Once testing passes:

1. Toggle the **"Active"** switch in top-right corner
2. The workflow will now run automatically at 9 AM daily

### Verify Activation

- Check that the toggle shows green/active
- You should see "Workflow activated" notification
- Tomorrow at 9 AM, check your outputs

---

## Customization

### Change Schedule Time

1. Click **"Daily at 9am"** node
2. Modify the cron expression:
   - `0 9 * * *` = 9:00 AM daily
   - `0 7 * * 1-5` = 7:00 AM weekdays only
   - `0 18 * * *` = 6:00 PM daily

### Add More Keywords

1. Open Google Sheet → Keywords tab
2. Add new rows with your keywords
3. Set Priority = "High" and Status = "Active"

### Change Output Location

To use a different Google Sheet:
1. Click "Read Keywords" node
2. Select new document/sheet
3. Repeat for "Save to Sheet4" node

---

## Troubleshooting

### Workflow Not Running

```
✓ Is the workflow toggled "Active"?
✓ Is your n8n instance running?
✓ Check Executions tab for errors
✓ Verify schedule trigger is configured correctly
```

### No Results from Exa

```
✓ Check API key is valid (test in Postman/curl)
✓ Try broader keywords
✓ Increase num_results from 3 to 5
✓ Extend date range (change 30 days to 60)
```

### Google Sheets Not Updating

```
✓ Re-authenticate Google OAuth
✓ Check document ID matches
✓ Verify "Sheet4" tab exists with correct headers
✓ Check column names match EXACTLY (case-sensitive)
```

### GPT Errors

```
✓ Check OpenAI API key is valid
✓ Verify account has API credits
✓ Check if you've hit rate limits
✓ The workflow has fallback logic - check if fallback engaged
```

### Telegram Not Sending

```
✓ Verify bot token is correct
✓ Confirm chat ID is your personal chat (not group)
✓ Make sure you've messaged the bot at least once
✓ Check Telegram credential in n8n
```

---

## Getting Help

- **n8n Community:** [community.n8n.io](https://community.n8n.io)
- **n8n Documentation:** [docs.n8n.io](https://docs.n8n.io)
- **This Repo Issues:** [GitHub Issues](../../issues)

---

## Next Steps

1. ✅ Workflow running successfully
2. 📖 Read [SCORING.md](./SCORING.md) to understand the methodology
3. 🎨 Read [CUSTOMIZATION.md](./CUSTOMIZATION.md) to adapt for your domain
4. 📈 Track your learning progress over time
5. 🔄 Iterate on keywords based on results quality

---

*Setup complete! You're now automatically building domain expertise.* 🎓
