# Family Meal Planner

Pick meals for the week, get a grocery list sorted by store section, and tag items by store.
This folder is a ready-to-host website. There is nothing to install or build.

## 1. Put it online with GitHub Pages (about 5 minutes)

1. On GitHub, create a new repository (for example `meal-planner`). Make it **Public**.
   GitHub Pages is free for public repositories.
2. Click **Add file → Upload files**, drag in **`index.html`**, **`app.js`** and **`recipes.json`**, then click **Commit changes**.
3. Open **Settings → Pages**. Under **Build and deployment**, set **Source** to **Deploy from a branch**,
   choose branch **main** and folder **/ (root)**, then **Save**.
4. Wait a minute or two. Your app will be at `https://YOUR-USERNAME.github.io/meal-planner/`.
5. On your phone, open that address, then use **Share → Add to Home Screen**.

## 2. Share the week and the grocery list with your partner (about 5 minutes, one time)

Without this step each phone keeps its own separate copy. With it, the meals, the week's picks, the
grocery list, check-offs and store tags are shared. Each change is saved on its own, so two people
editing at once don't overwrite each other.

**Create a free Firebase database**
1. Go to https://console.firebase.google.com and sign in with a Google account.
2. **Add project**, give it any name, and turn off Google Analytics (you don't need it).
3. In the left menu choose **Build → Realtime Database → Create Database**. Pick a location and choose
   **Start in locked mode**.
4. Open the **Rules** tab, replace everything with the rules below, and click **Publish**.
5. Open the **Data** tab and copy the database address at the top. It looks like
   `https://your-project-default-rtdb.firebaseio.com`.

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "households": {
      "$code": {
        ".read": "$code.length >= 12",
        ".write": "$code.length >= 12"
      }
    }
  }
}
```

**Connect the app**
1. In the app, open **Settings** (the people icon on the Meals screen) and find **Share with your household**.
2. Paste the database address, tap **Generate a code**, then tap **Turn on sharing**.
3. Tap **Copy invite link** and send it to your partner by text. When they open it on their phone,
   they join automatically. A small "Shared · up to date" line at the top of the app confirms it's working.

The household code works like a private password: only someone with the invite link (or the code) can
see or change the shared data, so send the link privately. If it ever leaks, turn sharing off, generate
a new code, and send a new link.

## How recipes get added

The recipe library is the file **`recipes.json`**. Nobody adds recipes from inside the app, so every phone
shows the same meals. To add meals:

1. Send recipe links to Claude and ask for them to be added. Tell Claude your site address
   (`https://YOUR-USERNAME.github.io/meal-planner/`) so it can read your current `recipes.json` and keep
   everything that's already in it.
2. Claude gives you an updated **`recipes.json`**.
3. In your GitHub repository, upload it over the old one (**Add file → Upload files**).
4. After a minute or two, reload the app. The new meals appear on every phone.

You never need to touch `app.js` to add recipes. Ratings, last-made dates, the week's picks and the
grocery list are stored separately from the recipes, so updating `recipes.json` never loses them.
If a recipe is taken out of the file, it disappears from the list but its ratings are kept, and they
come back if the recipe returns.

Each recipe in `recipes.json` looks like this (Claude writes these for you):

```json
{
  "id": "chicken-and-rice-taco-skillet",
  "name": "Chicken and Rice Taco Skillet",
  "servings": 4,
  "source": "https://thedefineddish.com/chicken-and-rice-taco-skillet/",
  "image": "https://.../photo.jpg",
  "ingredients": [{ "name": "Rice", "qty": 1, "unit": "cup", "aisle": "Pantry" }],
  "instructions": ["Step one.", "Step two."]
}
```

Keep each recipe's `id` the same forever, because ratings are filed under it.
If the file is ever missing or broken, the app keeps working from the last copy it saved and shows a
warning if it has none. It will not overwrite your data.

Built-in meals can't be edited or deleted in the app. (If you ever want people to add recipes from
inside the app, set `ALLOW_ADDING` to `true` near the top of `app.js`. Importing from links or photos then
needs an Anthropic API key, entered in Settings.)

## Good to know

- **"Cooking for" starts at 5.** Every recipe's amounts and the grocery list scale to that number. Change it
  with the stepper on the This week screen (1 to 12). To change the starting number for everyone, edit
  `DEFAULT_HOUSEHOLD` near the top of `app.js`.

- **Offline is fine.** Changes are saved on the phone first and sync when the connection is back.
- **Kid names** start blank so they aren't published in a public repository. Set them in Settings.
  They are shared with your partner once sharing is on.
- **Backup:** Settings has **Download backup** and **Restore from file**. Restoring a backup while
  sharing is on replaces what's shared.
- Recipe photos load from the recipe site's own server.
- The page loads React and the icon set from `esm.sh`, so it needs an internet connection.
