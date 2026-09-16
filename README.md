# Local AI Email Auto-Responder

This project runs a fully local email auto-responder with n8n and Ollama.

## Files

- `docker-compose.yml` defines the n8n and Ollama services, shared network, volumes, and health checks.
- `workflow.json` contains the exported n8n workflow.
- `.env.example` documents the required environment variables.
- `submission.json` provides the required evaluation credential schema.

## Startup

1. Copy `.env.example` to `.env` and fill in your email and n8n settings.
2. Run `docker compose up -d --build`.
3. Wait for both services to report healthy.
4. Open `http://localhost:5678` and import `workflow.json` into n8n.

## Model Availability

The Ollama container is configured to start the server and pull `llama3:8b` automatically if it is missing.

## Workflow Notes

- The trigger node uses IMAP and watches `INBOX` for unread messages.
- The IF node blocks self-replies and messages already marked with the auto-reply prefix.
- The HTTP node posts a JSON prompt to `http://ollama:11434/api/generate`.
- The SMTP node replies using the original sender and preserves threading with `messageId`.