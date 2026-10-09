# Family Meal Planner

A phone-and-computer web app for planning dinners and shopping. It lives on GitHub Pages.

## Files

- `index.html`, `app.js`: the app itself.
- `recipes.json`: the meal library (every recipe, with amounts, prep notes and steps).
- `images/`: photos for the cookbook recipes. Keep these in this folder.

To update the site, upload the changed files to the repository (**Add file → Upload files**), drag the
`images` folder in if photos changed, and click **Commit changes**. Give it a minute or two, then reload.

## Adding or changing recipes

Recipes are built outside the app and shipped in `recipes.json`, so everyone sees the same meals. Send the
recipe links to Claude, get back an updated `recipes.json`, and upload it. Ratings, picks and the grocery
list live in the shared database, so updating recipes never loses them.

Each recipe in `recipes.json` looks like this:

```json
{
  "id": "chicken-and-rice-taco-skillet",
  "name": "Chicken and Rice Taco Skillet",
  "servings": 4,
  "source": "https://example.com/recipe/",
  "image": "images/photo.jpg",
  "ingredients": [{ "name": "Yellow onion", "qty": 0.5, "unit": "", "aisle": "Produce", "prep": "finely diced (1 cup)" }],
  "instructions": ["Step one.", "Step two."]
}
```

Keep each recipe's `id` the same forever, because ratings are filed under it.

## Sharing between phones

There is no Settings page. Every phone and computer that opens the site shares the same week plan, grocery
list, ratings and last-made dates automatically, through this Firebase Realtime Database:

`https://collison-meal-planner-default-rtdb.firebaseio.com`

The household code (the folder name inside the database) is the first 20 characters of the SHA-256 of
`family-meal-planner-v1|https://collison-meal-planner-default-rtdb.firebaseio.com`, which is `85cfe0d597f72a2576d8`. It is built into `app.js`, so rebuilding the app always
gives the same one. Keep the database rules locked to that one folder (and the matching `photos` folder, which
holds photos people have changed):

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "households": {
      "85cfe0d597f72a2576d8": { ".read": true, ".write": true }
    },
    "photos": {
      "85cfe0d597f72a2576d8": { ".read": true, ".write": true }
    }
  }
}
```

(Firebase console → Realtime Database → Rules → paste → Publish.)

If a phone can't reach the database, the app keeps working, saves changes on that phone, and shows a small
notice. It catches up on its own when the connection returns.

## Good to know

- **Recipes are shown as written.** Each meal keeps its original serving size and ingredient amounts, with how
  to prepare each ingredient next to its name. The grocery list adds the recipes up exactly as written.
- **Shopping.** Check items off and they're crossed out and drop to the bottom of their section. **Done
  shopping** at the top of the Groceries page removes everything that's checked. An **Undo** button shows for
  about 10 seconds, and **Bring back** stays on the page afterwards. **Start a new week** resets it all.
- **Changing a meal's photo.** Open a meal and tap **Change photo** to pick one from the phone or take a new
  one. It is shrunk, saved on that phone, and shared with every device. **Use the original photo** puts the
  recipe's own photo back.
- **Works on any screen.** Bottom tabs on a phone; a side menu and wider layouts on a laptop or desktop.
- **Offline is fine.** Changes are saved on the phone first and sync when the connection is back.
- The page loads React and the icons from `esm.sh`, so it needs an internet connection.
