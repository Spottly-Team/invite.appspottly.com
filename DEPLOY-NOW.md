# Fix rapido — invite.appspottly.com dà 404

Il dominio è su Netlify ma **i file del sito non sono stati caricati** (su GitHub c’è ancora solo `prova`).

## Metodo più veloce (5 minuti, senza Git)

1. Vai su **https://app.netlify.com/drop**
2. Trascina l’intera cartella `invite-site` (questa cartella) nella finestra
3. Quando il deploy è verde, clicca il sito → **Domain management**
4. **Add domain** → `invite.appspottly.com` (se non c’è già)
5. Apri `https://invite.appspottly.com/testuser` → deve comparire la pagina nera con @testuser

## Metodo GitHub (dopo aver sistemato login)

GitHub non accetta più la password nel terminale. Usa **uno** di questi:

### A) Token (consigliato)
1. GitHub → Settings → Developer settings → **Personal access tokens**
2. Crea token con permesso `repo`
3. Nel terminale:
```bash
cd invite-site
git push https://TUO_USERNAME:IL_TOKEN@github.com/Spottly-Team/invite.appspottly.com.git main --force
```

### B) SSH
```bash
cd invite-site
git remote set-url origin git@github.com:Spottly-Team/invite.appspottly.com.git
git push -u origin main --force
```
(Serve chiave SSH aggiunta al account GitHub)

Poi su Netlify: **Trigger deploy** sul sito collegato al repo.

## Verifica

Questi URL devono funzionare (non 404):

- https://invite.appspottly.com/
- https://invite.appspottly.com/mario
- https://invite.appspottly.com/.well-known/apple-app-site-association

## Android App Links

In `.well-known/assetlinks.json` metti il SHA-256 da Play Console, poi ridistribuisci.
