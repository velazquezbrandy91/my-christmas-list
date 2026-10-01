# My Christmas List Website

This is a single-file static website. It requires no database, server, or paid service.

## Free hosting
Upload `index.html` to GitHub Pages, Netlify, or another static host.

## Built-in rules
- Family -> Mom/Dad/Sister/Grandparent -> budget
- Friends, Nieces & Nephews, Secret Santa, Edgar -> budget
- Multiple budget selections are supported
- Nieces & Nephews: priced gifts over $25 are hidden
- Secret Santa: priced gifts over $45 are hidden
- Homemade and Experiences are available on every page
- Special items follow the person-specific instructions from the supplied list
- Gift cards are available across tabs
- Items without a supplied price are displayed in an unpriced note rather than assigned to a budget

## Shared “Purchased” check-off setup

The website uses a free Firebase Realtime Database so a gift marked purchased disappears for everyone, not just the person who clicked it.

1. Go to https://console.firebase.google.com/ and create a new Firebase project (the free Spark plan is enough for this small site).
2. In the project, open **Build → Realtime Database** and create a database.
3. Choose a nearby location and start in **locked mode**.
4. Open the **Rules** tab and replace the rules with:

```json
{
  "rules": {
    ".read": true,
    ".write": true,
    "purchased": {
      "$item": {
        ".validate": "newData.val() === true"
      }
    }
  }
}
```

5. Copy the database URL shown by Firebase. It will look similar to `https://YOUR-PROJECT-default-rtdb.firebaseio.com`.
6. In `index.html`, find:

```js
const FIREBASE_DB_URL = '';
```

and put your database URL between the quotes.
7. Upload the updated `index.html` to GitHub Pages and commit it.

The Firebase URL is not a secret. The database rules intentionally allow visitors to read the purchased list and mark an item as purchased. They can only write `true` under `/purchased/` with the rules above.
