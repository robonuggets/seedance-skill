# Seedance Skill

> Turn any app into a cinematic motion graphics promo video using AI.

Seedance Skill is a Claude Code skill that generates glossy liquid glass motion graphics videos using ByteDance's Seedance 2.0 model via Fal AI. Give it screenshots of any app, and it produces Apple-keynote-quality promo clips.

## Examples

- Screenshot a SaaS dashboard -> 10-second liquid glass promo video
- Scrape App Store screenshots -> motion graphics ad for any iOS app
- Text prompt -> abstract motion identity sequence

## Quick Start

### 1. Install the skill

Copy the `SKILL.md` file to your Claude Code project's `.claude/skills/seedance/` directory.

### 2. Add your Fal AI key

```bash
echo "FAL_KEY=your-fal-ai-key" >> .env
```

Get a key at [fal.ai](https://fal.ai).

### 3. Generate

Tell Claude Code:

```
Generate a liquid glass motion graphics promo for https://linear.app
```

Claude will screenshot the site, upload the images, generate the video, and download it.

## What You Get

- **4-second test clips** for ~$1.21 (Pro, 720p)
- **10-second production promos** for ~$3.03 (Pro, 720p)
- Glossy liquid glass aesthetic with translucent panels, light refractions, and depth
- Multiple camera angles, floating devices, pure black backgrounds

## Prompting Patterns

The skill includes battle-tested prompt templates:

- **Liquid Glass** — translucent UI panels with reflections (the default)
- **Floating Desktop** — MacBook/iPad product shots in black void
- **Glass UI Ad** — close-ups on glass interactions
- **Abstract Motion Identity** — timestamped sequences with geometric transformations

## Sweet Spot

| Refs | Duration | Use Case | Cost (Pro 720p) |
|------|----------|----------|-----------------|
| 3 refs | 4s | Quick test | $1.21 |
| **5 refs** | **10s** | **Production promo** | **$3.03** |
| 1 ref | 4s 480p | Cheapest test | ~$0.81 |

**Golden rule: 1 reference image per 2 seconds of video.**

## Key Rules

1. **Never describe logos in prompts** — pass the actual logo image as a reference, or overlay in post
2. **Default to Pro tier** — only 25% more than Fast but noticeably better
3. **Use temp JSON files** for API calls — shell escaping breaks curl
4. **No text in video** — overlay text/logos in post-production for clean results
5. **Max 5 reference images** — more than that and quality degrades

## Pricing

Seedance 2.0 on Fal AI:

| Tier | Cost/sec (720p) |
|------|-----------------|
| Pro (default) | $0.30/sec |
| Fast (testing) | $0.24/sec |

## Built By

[RoboNuggets](https://robonuggets.com) — AI agent tools and community.

## License

MIT
