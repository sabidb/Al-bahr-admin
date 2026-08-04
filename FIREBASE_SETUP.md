# Firebase security setup — Broast Albahr

Do this once before rolling the new admin app to staff.
Takes about 5 minutes in the Firebase Console.

Everything below happens in the Firebase Console at
<https://console.firebase.google.com/project/broast-al-bahr>.

---

## 1. Enable email/password sign-in

1. Left sidebar → **Build → Authentication**
2. Click **Get started** (only shown the first time)
3. **Sign-in method** tab → **Email/Password** → toggle **Enable** → **Save**

---

## 2. Create the owner account

1. **Authentication → Users** tab → **Add user**
2. Email: your chosen owner email (e.g. `owner@yourdomain.com`)
3. Password: strong password of your choice — **write it down; you'll use it to log into the admin app**
4. **Add user**
5. From the users list, **copy the User UID** (looks like `k3f9…zA2`). Keep this on your clipboard for the next step.

> Real inbox recommended for the owner — password reset emails go there.
> Branch manager accounts get created from inside the admin app later, so skip them here.

---

## 3. Create the owner's role document in Firestore

The rules only recognise a signed-in user as an owner if there is a matching profile doc.

1. **Build → Firestore Database → Data** tab
2. **Start collection** (or if `users` collection already exists, open it)
3. **Collection ID**: `users` → **Next**
4. **Document ID**: paste the UID you copied in step 2
5. Add these fields:
   - `role` (string) → **`owner`**
   - `email` (string) → the owner email you used
   - `createdAt` (timestamp) → click the current-time button
6. **Save**

---

## 4. Publish the security rules

1. **Firestore Database → Rules** tab
2. Replace all text in the editor with the contents of `firestore.rules` in this repo
3. **Publish**
4. You should see "Rules published successfully"

> **Test it worked**: try opening the customer ordering site (broast-albahr) — the menu should still load. Try opening the admin app in a fresh browser window — orders should be **empty** until you log in.

---

## 5. Log into the admin app

1. Open the admin app in your browser
2. Enter the **owner email + password** you created in step 2
3. You should land on the **Home** dashboard with all 3 outlets

---

## 6. Create branch manager accounts

Now that you're logged in as owner, you can add the 3 branch managers from inside the app — no more console clicks.

1. In the admin app → **⚙️ Settings** → scroll to **👥 Staff Management**
2. For each branch, click **➕ Add Manager**
   - Pick the branch
   - Enter their email (can be real or `kakkiyah@yourdomain.com` style — up to you)
   - Set a starting password (they can change it later)
3. Hand each manager their email + password

---

## Notes / limitations

- **The POS ordering page (broast-albahr) still works without login** — it's public. It can only create orders and read the menu. It cannot read anyone else's orders or customer data.
- **Anti-spam on public order creation**: Firestore rules alone can't rate-limit. If spam becomes an issue, we can add [App Check](https://firebase.google.com/docs/app-check) later.
- **Password reset**: Owner can trigger a reset email for any staff member from the Staff Management panel (only works if the account uses a real email address).
- **Removing a staff member**: Delete their profile doc from Staff Management → they lose access on next request. Also delete their Auth user from the Firebase console for full cleanup.
- **The old `settings/accounts` document** (from the previous client-password login) is no longer used. Safe to ignore or delete manually from Firestore.
