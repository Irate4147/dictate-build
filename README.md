# dictate-build

Automatiske, uændrede builds af [Dictate Keyboard](https://github.com/DevEmperor/DictateKeyboard) (Apache-2.0), signeret med egen nøgle, så appen kan installeres og auto-opdateres via [Obtainium](https://github.com/ImranR98/Obtainium) i stedet for Google Play.

Repoet indeholder ingen app-kode – kun en workflow (`.github/workflows/build.yml`), der:

1. dagligt tjekker upstream for et nyt release-tag (`vX.Y.Z`),
2. bygger det (NDK, sherpa-onnx, Bergamot, Gradle) på GitHub Actions,
3. signerer med nøglen i repoets secrets og udgiver en release med APK + SHA256SUMS.

Al udvikling og vedligeholdelse sker upstream.

## Opsætning (én gang)

1. Tilføj secrets under *Settings → Secrets and variables → Actions*: `KEYSTORE_BASE64`, `KEYSTORE_PASSWORD`, `KEY_ALIAS`, `KEY_PASSWORD`.
2. *Actions → Byg Dictate fra upstream → Run workflow* for at lave første build (tager ca. 20–40 min).
3. Obtainium på telefonen: *Add app* → URL `https://github.com/<bruger>/dictate-build` → gem. Obtainium opdaterer automatisk, når der kommer en ny release.

## Bemærkninger

- Samme pakkenavn som Play-versionen (`net.devemperor.dictate`) – afinstallér Play-versionen først; de to kan ikke opdatere hinanden (forskellig signatur).
- Kildekoden er FOSS, men buildet trækker også Googles lukkede biblioteker ind (Play Billing, play-services-wearable, ML Kit OCR), præcis som upstream. Dictate Cloud-køb virker ikke uden Play; BYOK virker normalt.
- Mister du nøglen, kan nye builds ikke installeres som opdatering.
