# Deploy Skill — Spec (v0)

## Goal
Given a local project directory (usually a Next.js tool site) and optional DNS/provider settings, deploy the site and return the **live URL**.

Default target is **Vercel**.

## Inputs
Required:
- `projectDir`: string (absolute path to local repo)
- `domain`: string — apex domain to configure (e.g. `example.com`)

Optional:
- `provider`: enum = `vercel|cloudflare|vps` (default: `vercel`)
- `vercelAccount`: string (default: `valueaddrei-beep`)  
  - If team scope is used, provide `vercelTeam` instead.
- `vercelTeam`: string (optional)
- `cloudflareZone`: string (optional) — if provider=cloudflare
- `vpsHost`: string (optional) — if provider=vps

## Outputs
- `deploymentUrl`: string (required) — e.g. https://xxx.vercel.app
- `provider`: string
- `dnsRecords`: array (required when provider=vercel)
  - Return **what the user should set in Cloudflare DNS** for both apex and www.
  - For Vercel (typical):
    - apex (`@`) → **A** `76.76.21.21`
    - `www` → **CNAME** `cname.vercel-dns.com`
  - Always label:
    - `name` (e.g. `@`, `www`)
    - `type` (`A|CNAME`)
    - `value`
    - `ttl` (suggested)
    - `proxy` recommendation (for Cloudflare)
- `notes`: bullets including:
  - whether build succeeded
  - any human steps needed (login/2FA)
  - domain status (attached in Vercel or pending DNS)

## Non-goals
- No app-specific feature dev (only deploy)
- No GA/GSC setup (separate skills)

## Execution guidance
- Prefer deterministic CLI deploys.
- If auth required (Vercel login, Cloudflare login, SSH keys), pause with `needs_human` style instruction in chat.
- Always verify with a quick HTTP open (or at least report the final URL from CLI).

## Suggested Steps (state machine)
1) `preflight`
   - confirm `projectDir` exists and is a git repo
   - run `npm install` (if needed) and `npm run build`
2) `deploy`
   - provider=vercel: `vercel --prod` (or `vercel deploy --prod`) with correct scope
   - provider=cloudflare: (future) pages/workers deploy
   - provider=vps: (future) rsync/docker/systemd
3) `attach_domain` (optional)
4) `finalize`
   - return deployment URL + notes
