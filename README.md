# Family Meal Planner

Pick meals for the week, get a grocery list sorted by store section, and tag items by store.
This folder is a ready-to-host website. There is nothing to install or build.

## Put it online with GitHub Pages (about 5 minutes)

1. On GitHub, create a new repository (for example `meal-planner`). Make it **Public**.
   GitHub Pages is free for public repositories.
2. Click **Add file → Upload files**, drag in **`index.html`** and **`app.js`**, then click **Commit changes**.
3. Open **Settings → Pages**. Under **Build and deployment**, set **Source** to **Deploy from a branch**,
   choose branch **main** and folder **/ (root)**, then **Save**.
4. Wait a minute or two. Your app will be at `https://YOUR-USERNAME.github.io/meal-planner/`.
5. On your phone, open that address, then use **Share → Add to Home Screen**.

## Good to know

- **Your meals are saved in the browser you use.** Your phone and your laptop each keep their own copy.
  To move meals between devices or keep a safety copy, open **Settings** (the people icon on the Meals
  screen) and use **Download backup** and **Restore from file**.
- **Kid names** start blank so they aren't published in a public repository. Set them in Settings.
- **Importing recipes from links or photos** and **Find photos** use Claude. To turn them on, open
  Settings and paste your own Anthropic API key. It is stored only in your browser and sent only to Anthropic.
  Usage is billed to your Anthropic account. Never put a key into `app.js` or any file in the repository.
  Without a key, everything else still works: typing meals in, ratings, the week plan, and groceries.
- The 20 built-in recipe photos and any photo link you paste load from the recipe site's own server.
- The page loads React and the icon set from `esm.sh`, so it needs an internet connection.
- If imports stop working one day because the AI model name was retired, change the `MODEL` line near
  the top of `app.js`.
