# Extended Details

## accounts.json Format

Store profile configuration in project root:

```json
{
  "active": "https://www.tiktok.com/@example_creator",
  "profiles": [
    {
      "url": "https://www.tiktok.com/@example_creator",
      "name": "example_creator",
      "added": "2024-01-21"
    },
    {
      "url": "https://www.tiktok.com/@techcreator",
      "name": "techcreator",
      "added": "2024-01-21"
    }
  ]
}
```

## Creating/Reading accounts.json

To read:
```bash
cat accounts.json 2>/dev/null || echo '{"active": null, "profiles": []}'
```

To write (use Python for proper JSON handling):
```bash
python3 -c "
import json
from pathlib import Path

accounts_file = Path('accounts.json')
if accounts_file.exists():
    data = json.loads(accounts_file.read_text())
else:
    data = {'active': None, 'profiles': []}

# Modify data as needed...
# data['active'] = 'NEW_URL'
# data['profiles'].append({'url': 'URL', 'name': 'NAME', 'added': 'DATE'})

accounts_file.write_text(json.dumps(data, indent=2))
print('Saved!')
"
```

## Integration with Other Commands

When `/bulk` or `/transcribe` runs, check for active profile:

1. If `accounts.json` exists and has an active profile, offer to use it → verify: step output matches expected outcome
2. Otherwise, ask for profile URL as normal → verify: user confirms

## Validation

Before adding/switching, validate the profile:

```bash
source .venv/bin/activate && python -c "
from short_form_scraper.scraper.tiktok import TikTokScraper

url = 'PROFILE_URL_HERE'
scraper = TikTokScraper(url)

try:
    videos = list(scraper.get_video_urls(limit=1))
    if videos:
        print(f'Valid profile! Found videos.')
        print(f'Latest: {videos[0].title}')
    else:
        print('Profile exists but has no videos.')
except Exception as e:
    print(f'Error: Could not access profile - {e}')
"
```

## Example Session

User: `/accounts`

Claude:
**Manage TikTok Profiles**

What would you like to do?
1. Switch profile
2. Add profile
3. List profiles
4. Remove profile
5. Process multiple

User: 2

Claude: What TikTok profile URL do you want to add?

User: https://www.tiktok.com/@techreviewer

Claude:
[Validates profile]
Added @techreviewer to your profiles!

You now have 2 saved profiles. Run `/accounts` again to switch between them or process multiple at once.
