# ctfd-pages-theme

- Original repo: https://github.com/frankli0324/ctfd-pages-them

## Description

- Organizes challenges into separate sections with a left-side menu, replacing the single-page challenge display from the core-beta theme.

- Ideal for environments with a large number of challenges.

- Demo screenshot:
  
  <img width="1659" height="867" alt="image" src="https://github.com/user-attachments/assets/d49550d2-ef9d-4b61-8aa5-c6c7048d154b" />


## Installation

- Requires a pre-installed CTFd instance

```sh
git clone https://github.com/CTFd/CTFd # Skip if already installed
git clone https://github.com/ndhoc/ctfd-pages-theme CTFd/themes/pages
```

- Navigate to  `CTFd/CTFd/api/v1/challenges.py`

- Add this import at the top of the file:

```py
from CTFd.cache import cache
```

- Then add the following route:

```py
@challenges_namespace.route("/categories")
class ChallengeCategories(Resource):
    @challenges_namespace.doc(description="Endpoint to get Challenge categories in bulk")
    @cache.memoize(timeout=60)
    def get(self):
        chal_q = (Challenges.query.with_entities(Challenges.category).group_by(Challenges.category))
        if not is_admin() or request.args.get("view") != "admin":
            chal_q = chal_q.filter(and_(Challenges.state != "hidden", Challenges.state != "locked"))
        return {"success": True, "data": [i.category for i in chal_q]}
```


- Verify the endpoint /api/v1/challenges/categories returns expected results:

```json
{"success": true, "data": ["pwn", "web"]}
```

- Installation tutorial video (Author: Frank0Li0)

[![Video Demo](https://img.youtube.com/vi/1Wqgjok4i88/maxresdefault.jpg)](https://www.youtube.com/watch?v=1Wqgjok4i88)

## Usage

1. Go to `Admin Panel` > `Themes`
2. Select `pages` theme
3. Enjoy!

