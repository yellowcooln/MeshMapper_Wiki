# Webhooks

Webhooks allow MeshMapper to send real-time notifications to any external HTTPS endpoint when events occur in your region. This is useful for integrating with services like Home Assistant, custom bots, or any system that can receive HTTP POST requests.

Webhooks work alongside Discord DM notifications — enabling a webhook does not disable Discord notifications.

## Setup

1. Navigate to the **Admin Portal** → **Settings** tab for your region.
2. In the **Webhook Notifications** section, enter your webhook URL.
3. Select which events should trigger a webhook notification.
4. Click **Save Settings**.
5. Use the **Send Test** button to verify your endpoint receives the test payload.

!!! warning "HTTPS Required"
    Only HTTPS URLs are accepted. HTTP, internal, and private network URLs are blocked for security.

## Events

You can enable or disable each event type individually:

| Event | Description |
|-------|-------------|
| **Duplicate Repeater ID** | A new repeater has been detected with an ID that collides with an existing repeater, putting both into Excluded status. |
| **Pending Repeater** | Your region has repeaters in Pending state awaiting admin review. Checked once daily. |
| **Offline Observer** | One or more MQTT observers in your region may be offline. Checked once daily. |
| **Visitor Message** | A visitor has sent a message to your region's administrators via the map. |

By default, all events are enabled when a webhook URL is configured.

## Payload Format

Every webhook sends an HTTP POST request with a JSON body. The structure is consistent across all event types:

```json
{
  "event": "duplicate_repeater",
  "region": "YOW",
  "timestamp": 1744123456,
  "message": "New ID collision detected on YOW.\n\nCollision Group: A1\n\nRepeaters:\nRepeaterA (A1B2C3D4)\nRepeaterB (A1E5F6G7)",
  "data": {
    "collision_group": "A1",
    "repeaters": ["RepeaterA (A1B2C3D4)", "RepeaterB (A1E5F6G7)"]
  }
}
```

### Fields

- **event** — The event type (e.g., `duplicate_repeater`, `pending_repeater`, `offline_observer`, `visitor_message`, `test`).
- **region** — The region code (e.g., `YOW`).
- **timestamp** — Unix timestamp of when the event occurred.
- **message** — A human-readable summary of the event. This is the same text sent via Discord DM, so simple integrations can use it directly.
- **data** — Event-specific structured data (see below).

### Event-Specific Data

**duplicate_repeater:**

- `collision_group` — The ID that collided (e.g., `A1`).
- `repeaters` — Array of repeater names and IDs involved in the collision.

**pending_repeater:**

- `count` — Number of repeaters currently in Pending state.

**offline_observer:**

- `stale_observers` — Array of observer descriptions that appear to be offline.

**visitor_message:**

- `body` — The full text of the visitor's message.

**test:**

- `info` — A static test string confirming the webhook is working.

## Troubleshooting

**Webhook not firing:**

- Ensure the URL starts with `https://`.
- Verify the event type checkbox is enabled in Settings.
- Check that the URL is reachable from the MeshMapper server.
- Use the **Send Test** button to isolate the issue.

**Test button shows "Failed":**

- The endpoint may be unreachable, returning an error status code, or timing out (3-second limit).
- Verify the URL is correct and the endpoint is accepting POST requests with JSON content.

**Receiving duplicate notifications:**

- Webhooks and Discord DMs operate independently. If you receive both, this is expected behavior.

## Technical Details

- Webhook requests use HTTP POST with `Content-Type: application/json`.
- Requests have a 3-second timeout and do not follow redirects.
- Failed webhook deliveries do not retry.
