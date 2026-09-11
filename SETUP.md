# Setup & Customization

Source for the **GitHub profile README** shown at <https://github.com/chib30333>.

The repo is already created and pushed, so day-to-day work is just: edit
`README.md`, commit, push.

```bash
git add .
git commit -m "Update profile README"
git push
```

---

## 1. Turn on the stats images (required once)

The stats and contribution calendar are **not** fetched from a public service
any more. The old `github-readme-stats.vercel.app` / `github-profile-trophy` /
`github-readme-activity-graph` cards were broken because those shared instances
are permanently over their Vercel quota — nothing you can fix from your side.

Instead, [`.github/workflows/metrics.yml`](.github/workflows/metrics.yml) renders
four SVGs and commits them into `assets/` in this repo. The README points at
your own `raw.githubusercontent.com` URLs, so they can never rate-limit or 404
once generated.

Until the workflow has run once, those two images will show as broken. To set it up:

### a. Create a personal access token

1. <https://github.com/settings/tokens> → **Generate new token (classic)**
2. Name it `metrics`, expiry `No expiration` (or set a reminder to rotate it)
3. Tick **`public_repo`** only — nothing else is needed for public stats
4. **Generate token** and copy it

### b. Add it as a repository secret

1. This repo → **Settings → Secrets and variables → Actions → New repository secret**
2. Name: `METRICS_TOKEN`
3. Value: the token you just copied → **Add secret**

### c. Allow the workflow to commit

**Settings → Actions → General → Workflow permissions** → select
**Read and write permissions** → **Save**.

### d. Run it

**Actions** tab → **GitHub Metrics** → **Run workflow**.

It commits `assets/metrics.light.svg`, `assets/metrics.dark.svg`,
`assets/isocalendar.light.svg`, `assets/isocalendar.dark.svg`, then refreshes
itself daily at 00:00 UTC.

> If the run fails with a token error, the secret name is wrong or the token is
> missing `public_repo`. If it fails on the commit step, step (c) was skipped.

---

## 2. What to edit in `README.md`

Every spot needing your input is marked with `<!-- EDIT ME -->`.

| Section | What to change |
| --- | --- |
| Tech Stack | Delete badges you do not use; add more from [simpleicons.org](https://simpleicons.org) |
| Featured Projects | Replace `project-one` / `project-two` / `project-three` with real repo names, or delete the section and use GitHub's pinned repositories |

### Adding a tech badge

```markdown
![Name](https://img.shields.io/badge/Name-HEXCOLOR?style=for-the-badge&logo=SLUG&logoColor=white)
```

`SLUG` is the icon name from <https://simpleicons.org> in lowercase with no
spaces or dots — e.g. `csharp`, `dotnet`, `nodedotjs`, `microsoftazure`,
`amazonwebservices`. In the label text, `#` must be written `%23` and a space
`%20` (that is why C# is `C%23`).

---

## 3. Light + dark mode

Both generated images use a `<picture>` element so GitHub swaps them with the
visitor's theme:

```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="...dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="...light.svg" />
  <img src="...light.svg" alt="..." />
</picture>
```

Keep that pattern if you add more images. The workflow generates each card twice
(`config_theme: classic` for light, `dark` for dark) to feed it.

---

## 4. Tuning the metrics cards

`metrics.yml` uses [lowlighter/metrics](https://github.com/lowlighter/metrics).
Useful knobs:

| Option | Effect |
| --- | --- |
| `config_timezone` | Currently `Asia/Seoul` — set to your own so streaks roll over at your midnight |
| `base` | Which stock sections render: `header, activity, community, repositories, metadata` |
| `plugin_languages_limit` | How many languages in the breakdown (currently 8) |
| `plugin_isocalendar_duration` | `full-year` or `half-year` |

There are ~40 more plugins (achievements, habits, stars, topics, WakaTime…) in
the [plugin catalogue](https://github.com/lowlighter/metrics#-plugins). Add one
as another step in the workflow, writing to its own `assets/*.svg`, then embed
it with the same `<picture>` block.

---

## 5. Services still used

Only these two, both reliable:

| Service | Used for |
| --- | --- |
| [shields.io](https://shields.io) | Tech-stack badges and repo star counts |
| [lowlighter/metrics](https://github.com/lowlighter/metrics) | Stats + contribution calendar, rendered by Actions into this repo |

---

## 6. Removed sections

Dropped on request — restore from git history (`git log -p README.md`) if wanted:

- Header banner, animated typing line, follower/repo/view badges
- About Me block
- Contribution Snake (workflow `snake.yml` deleted too)
- Dev Quote
- Connect With Me + footer banner
