# Square Appointments MCP server through Cotlet.dev

Connect Square Appointments to ChatGPT, Claude, Codex and Claude Code through Cotlet.dev. Review your salon calendar, prepare bookings and manage service availability in your signed-in Square Appointments account.

This is an unofficial integration, not affiliated with Square Appointments. Cotlet.dev hosts the connection through your Cloud Browser; this repository provides an optional Python stdio MCP bridge.

## Connect your account

1. [Request access to Square Appointments through Cotlet.dev](https://cotlet.dev/signup?app=square-appointments).
2. Open Square Appointments in your Cotlet.dev Cloud Browser, sign in yourself and enable it for agents.
3. Add Cotlet.dev to your agent using the connection details below.

### ChatGPT

Open Settings > Apps & Connectors > Create. Use `https://cotlet.dev/mcp/v1/connect` as the MCP server URL and choose OAuth.

### Claude

Open Settings > Connectors > Add custom connector and enter `https://cotlet.dev/mcp/v1/connect`.

### Codex

Create a Cotlet.dev API key in your console and set `COTLET_API_KEY` in your environment. Add this to your Codex configuration:

```toml
[mcp_servers.cotlet]
url = "https://cotlet.dev/mcp/v1"
bearer_token_env_var = "COTLET_API_KEY"
```

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

## Appointment workflows

Examples based on the [live Square Appointments integration reference](https://cotlet.dev/integrations/square-appointments-api-mcp-server):

- Review upcoming appointments for selected staff members.
- Prepare a proposed appointment with the customer, service, location and time for approval.
- Review appointment intervals, advance booking limits and daily booking limits.
- Update staff availability after approving the change to public booking slots.
- Manage bookable services and resources with the required Square permissions.
- Cancel or mark an appointment as no-show after confirming the client notification choice.

Representative operations from the live reference:


- `view_staff_calendars`
- `schedule_appointment`
- `appointment_booking_rules`
- `update_staff_availability`
- `manage_bookable_resources`
- `create_manage_services`
- `cancel_or_no_show_appointment`


The live page describes 68 Cloud Browser capabilities, including 7 read and 61 write capabilities. Hosted operation groups use `square_appointments_read` and `square_appointments_write`. Square has an official MCP server; check its documented Appointments coverage for your task. Seller-level Bookings API writes require Square Appointments Plus or Premium. Browser actions depend on the signed-in user's Square plan and permissions.

Creating a booking can send a text or email notification. Confirm bookings, public availability changes, messages, cancellations and payments before execution.

## Optional local stdio bridge

Python 3.10 or newer is required, with no runtime dependencies. Clone this repository and configure a client that supports stdio:

```json
{"mcpServers":{"square-appointments":{"command":"python3","args":["/absolute/path/to/square-appointments-mcp/square_appointments_agent.py"],"env":{"COTLET_API_KEY":"your-cotlet-key"}}}}
```

The bridge lists seven browser helpers: `square_appointments_guide`, `square_appointments_status`, `square_appointments_open`, `square_appointments_read`, `square_appointments_click`, `square_appointments_type` and `square_appointments_close`. These forward to Cotlet.dev's shared browser tools. They are separate from the hosted Square Appointments capabilities above. Call the guide first and close the browser when finished. Tool discovery works without a key; executing a tool requires your Cotlet.dev key and connected account.

```bash
python3 square_appointments_agent.py connect
python3 square_appointments_agent.py guide
python3 square_appointments_agent.py status
python3 square_appointments_agent.py serve
```

You can also build the included Dockerfile and run it with `COTLET_API_KEY` supplied as an environment variable.

## Reference and access

- [Square Appointments connection guide and operation reference](https://cotlet.dev/integrations/square-appointments-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=square-appointments)
- [Request access](https://cotlet.dev/signup?app=square-appointments)
- [Cotlet.dev console](https://cotlet.dev/app)

Licensed under Elastic License 2.0. See LICENSE.
