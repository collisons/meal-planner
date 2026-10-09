# Family Meal Planner

Pick meals for the week, get a grocery list sorted by store section, and tag items by store.
This folder is a ready-to-host website. There is nothing to install or build.

## 1. Put it online with GitHub Pages (about 5 minutes)

1. On GitHub, create a new repository (for example `meal-planner`). Make it **Public**.
   GitHub Pages is free for public repositories.
2. Click **Add file → Upload files**, drag in **`index.html`** and **`app.js`**, then click **Commit changes**.
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

## Good to know

- **Offline is fine.** Changes are saved on the phone first and sync when the connection is back.
- **Kid names** start blank so they aren't published in a public repository. Set them in Settings.
  They are shared with your partner once sharing is on.
- **Importing recipes from links or photos** and **Find photos** use Claude. To turn them on, open
  Settings and paste your own Anthropic API key. It is stored only in your browser and sent only to
  Anthropic. Usage is billed to your Anthropic account. Never put a key into `app.js` or any file in the
  repository. Only one of you needs a key, because imported meals are shared. Without a key, everything
  else still works.
- **Backup:** Settings also has **Download backup** and **Restore from file**. Restoring a backup while
  sharing is on replaces what's shared.
- The 20 built-in recipe photos and any photo link you paste load from the recipe site's own server.
- The page loads React and the icon set from `esm.sh`, so it needs an internet connection.
- If imports stop working one day because the AI model name was retired, change the `MODEL` line near
  the top of `app.js`.
