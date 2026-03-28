# `🏆` Trophy

<img width="1000em" src="https://github-profile-trophy.vercel.app/?username=Mephisto-Grumpy&theme=onedark" />

# `📊` Statistic

<img width="450em" src="https://streak-stats.demolab.com/?user=Mephisto-Grumpy&theme=dark" />
<img width="450em" src="https://readme-stats.pungrumpy.com/api?username=Mephisto-Grumpy&show_icons=true&include_all_commits=true&count_private=true&hide_border=true&theme=dark" />
<img width="450em" src="https://readme-stats.pungrumpy.com/api/top-langs/?username=Mephisto-Grumpy&layout=compact&custom_title=Most%20used%20languages&langs_count=10&include_all_commits=true&hide_progress=false&hide_border=true&theme=dark&hide=">

# `🛬` Contributions

<a href="./profile-3d-contrib/profile-night-green.svg">
  <img width="900em" src="./profile-3d-contrib/profile-night-green.svg">
</a>

# `🚀` Docker deployment plan (Coolify)

1. Build and deploy this repository through Coolify using a `Dockerfile` source.
2. Use the included `docker-compose.yml` for local/Coolify-compatible service wiring:
   ```yaml
   services:
     app:
       build: .
       environment:
         - PORT=${PORT:-3000}
       ports:
         - "${PORT:-3000}:3000"
       healthcheck:
         test: ["CMD-SHELL", "wget -qO- http://127.0.0.1:$${PORT:-3000}/healthz || exit 1"]
         interval: 30s
         timeout: 5s
         retries: 3
         start_period: 20s
   ```
3. Add an HTTP health endpoint in the app (example: `/healthz`) and configure Docker health checks:
   ```dockerfile
   HEALTHCHECK --interval=30s --timeout=5s --start-period=20s --retries=3 \
     CMD wget -qO- http://127.0.0.1:${PORT:-3000}/healthz || exit 1
   ```
   > Use whichever HTTP client exists in your image (`wget` or `curl`) and install it in the Docker image if needed.
4. In Coolify, map the service domain and set the same health check path (`/healthz`) so failed containers are restarted automatically.
5. Use environment variables in Coolify for runtime configuration and secrets.
6. Enable auto-deploy from the default branch after successful build and health check.

# `🔔` Notifications: switch from Telegram to Discord

- Replace Telegram-based notifications with a Discord incoming webhook.
- Store the webhook in Coolify as `DISCORD_WEBHOOK_URL` (secret).
- Send deployment/build alerts from your CI workflow, Coolify post-deploy hook, or deployment script using `discord.py`:
  > Install dependency first: `pip install discord.py`
  ```python
  import os
  import discord

  webhook_url = os.environ.get("DISCORD_WEBHOOK_URL")
  if not webhook_url:
      raise RuntimeError("DISCORD_WEBHOOK_URL is not set")

  webhook = discord.SyncWebhook.from_url(webhook_url)
  webhook.send("✅ Deployment succeeded")
  ```
- Optional: include environment, commit SHA, and health check status in the Discord message body.
