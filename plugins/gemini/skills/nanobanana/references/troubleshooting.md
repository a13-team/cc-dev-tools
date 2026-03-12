# Nanobanana Troubleshooting

## Authentication Methods

| Method | Env Vars Required | Best For |
|--------|-------------------|----------|
| `api_key` (default) | `NANOBANANA_API_KEY` or `GEMINI_API_KEY` | Local dev, simple setups |
| `vertex_ai` | `GCP_PROJECT_ID`, `GCP_REGION` + ADC | Production on GCP |
| `auto` | Tries API key first, falls back to Vertex AI | Flexible environments |

**Vertex AI setup** (add to `.zshrc`):
```bash
export NANOBANANA_AUTH_METHOD=vertex_ai
export GCP_PROJECT_ID=your-project-id
export GCP_REGION=us-central1
```

**IAM requirement**: Grant `roles/aiplatform.user` to the service account or user.

---

## Error Reference

| Error | Cause | Fix |
|-------|-------|-----|
| `Skipping extension: An unknown error` | Missing config file | Reinstall: `rm -rf ~/.gemini/extensions/nanobanana && gemini extensions install https://github.com/gemini-cli-extensions/nanobanana` |
| `Generative Language API is disabled` | API not enabled in GCP | Enable at `console.developers.google.com/apis/api/generativelanguage.googleapis.com` |
| Extension not found | Not installed | `gemini extensions install https://github.com/gemini-cli-extensions/nanobanana` |
| API key missing | No auth configured | Set `GEMINI_API_KEY` or switch to `vertex_ai` auth |
| Vertex AI auth failed | Missing GCP vars or no ADC | Run `gcloud auth application-default login` and set env vars |
| IAM permission denied | Missing `roles/aiplatform.user` | Grant IAM role to service account |
| Model unavailable | Invalid model ID | Check `NANOBANANA_MODEL` value |
| Generation failed | Prompt safety filter | Rephrase the prompt |
| Quota exceeded | Rate limit hit | Wait for reset or switch to flash model |
| MCP bytestring encoding errors | Known bug in nanobanana MCP server | Do not use MCP mode. Use CLI mode only: `gemini --yolo -p "/command ..."` |
