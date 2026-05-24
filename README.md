# Invite site — GitHub + Netlify

Repo: [github.com/Spottly-Team/invite.appspottly.com](https://github.com/Spottly-Team/invite.appspottly.com)

Sito statico per `https://invite.appspottly.com/{username}`.

## Deploy su Netlify (metodo consigliato)

### 1. GitHub
Push della cartella `invite-site` nel repo (es. repo dedicato o monorepo).

### 2. Netlify
1. [app.netlify.com](https://app.netlify.com) → **Add new site** → **Import from Git**
2. Scegli il repo GitHub
3. Impostazioni build:
   - **Base directory:** `invite-site` (se il repo è la root SpottlyApp, altrimenti lascia vuoto se repo solo invite)
   - **Publish directory:** `.` (o `invite-site` se base directory è root repo)
   - **Build command:** *(vuoto — sito statico)*
4. **Deploy site**

### 3. Dominio custom
Netlify → **Domain management** → **Add custom domain** → `invite.appspottly.com`

Nel DNS del dominio `appspottly.com`:
- **CNAME** `invite` → `[tuo-sito].netlify.app`

Attendi propagazione DNS + certificato SSL (Netlify lo fa in automatico).

### 4. Android — SHA-256 obbligatorio
1. Play Console → Spottly → **Setup** → **App integrity**
2. Copia **SHA-256 certificate fingerprint** (release)
3. Sostituisci in `.well-known/assetlinks.json`:
   `"SOSTITUISCI_CON_SHA256_PLAY_CONSOLE"`
4. Commit + push → Netlify ridistribuisce

Verifica Android:
```text
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=https://invite.appspottly.com&relation=delegate_permission/common.handle_all_urls
```

### 5. iOS — Apple Developer
1. Identifiers → `com.spottlyapp.mobile` → abilita **Associated Domains**
2. Nuova build app con `associatedDomains` in `app.json` (già configurato)
3. Verifica:
   ```text
   https://invite.appspottly.com/.well-known/apple-app-site-association
   ```

### 6. Nuova build app
Universal/App Links richiedono build EAS (non Expo Go):
```bash
eas build --platform ios
eas build --platform android
```

## Struttura file

```text
invite-site/
├── netlify.toml          # rewrite /mario → invite.html + headers JSON
├── invite.html           # pagina invito + fallback store
├── index.html            # root → appspottly.com
└── .well-known/
    ├── apple-app-site-association
    └── assetlinks.json
```

## Test

| URL | Risultato atteso |
|-----|------------------|
| `invite.appspottly.com/mario` | Pagina invito @mario |
| Tap link con app installata | Apre profilo in Spottly |
| Tap link senza app | Redirect App Store / Play Store |
| `/.well-known/apple-app-site-association` | JSON valido |

## Note
- Il link condiviso nell'app è già `https://invite.appspottly.com/{username}`
- L'app risolve lo username su Firestore e apre `UserProfile`
- Se l'utente non è loggato, l'invite viene salvato e aperto dopo il login
