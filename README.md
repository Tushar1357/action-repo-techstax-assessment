# action-repo

This repository is configured to send GitHub webhooks on PUSH, PULL_REQUEST, and MERGE actions to the webhook-repo endpoint.

## Purpose

This repository demonstrates GitHub webhook integration by triggering events that are captured by the webhook-repo Flask application.

## Webhook Setup

### Step 1: Deploy webhook-repo

First, deploy the webhook-repo application to get a public URL. You can use:
- Heroku
- Railway
- Render
- ngrok (for local testing)

### Step 2: Configure GitHub Webhook

1. Go to this repository's **Settings**
2. Click on **Webhooks** in the left sidebar
3. Click **Add webhook**
4. Configure the webhook:
   - **Payload URL**: `https://your-webhook-domain.com/webhook`
   - **Content type**: `application/json`
   - **Secret**: (optional, leave empty for now)
   - **Which events would you like to trigger this webhook?**
     - Select "Let me select individual events"
     - Check: ✅ Pushes
     - Check: ✅ Pull requests
   - **Active**: ✅ Checked
5. Click **Add webhook**

### Step 3: Test the Integration

#### Test PUSH Event

```bash
echo "Test push event" >> sample.txt
git add sample.txt
git commit -m "Test push webhook"
git push origin main
```

#### Test PULL_REQUEST Event

```bash
git checkout -b test-branch
echo "Test PR" >> sample.txt
git add sample.txt
git commit -m "Test PR webhook"
git push origin test-branch
```

Then create a Pull Request on GitHub from `test-branch` to `main`.

#### Test MERGE Event

Merge the Pull Request created above. This will trigger a MERGE event.

## Viewing Events

Visit your webhook-repo UI at `https://your-webhook-domain.com` to see the events displayed in real-time.

The UI polls MongoDB every 15 seconds and displays events in the format:
- **PUSH**: "author" pushed to "branch" on timestamp
- **PULL_REQUEST**: "author" submitted a pull request from "from_branch" to "to_branch" on timestamp
- **MERGE**: "author" merged branch "from_branch" to "to_branch" on timestamp

## Troubleshooting

### Webhook Not Triggering

1. Check webhook delivery status in GitHub Settings → Webhooks
2. Click on the webhook and view "Recent Deliveries"
3. Check response codes and payloads

### Events Not Showing in UI

1. Verify MongoDB connection in webhook-repo
2. Check webhook-repo logs for errors
3. Ensure the webhook endpoint is publicly accessible

## Repository Structure

```
action-repo/
├── README.md          # This file
└── sample.txt         # Sample file for testing
```

## Next Steps

1. Deploy webhook-repo to a hosting platform
2. Configure the webhook URL in this repository
3. Test all three event types (PUSH, PULL_REQUEST, MERGE)
4. Monitor events in the webhook-repo UI
