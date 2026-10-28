# Fork stonosbv — status en takenlijst

Deze fork wordt bewust vastgezet en zelf onderhouden: we draaien alleen code die we nagelezen hebben.
Branch `stonos` = onze versie. `upstream` = taylorwilsdon/google_workspace_mcp.

## Stand (bijgewerkt 2026-10-28)

- Basis: **v2.1.0** (9 oktober 2026), de laatste release die minstens twee weken oud is.
- Nulmeting van v2.1.0: **geschikt met voorwaarden** (lokaal rapport in `reviews/`, niet in git).
- Eerste wekelijkse review (v2.1.1 t/m v2.2.1): nog niet rebasen.
- `pip-audit` op de lockfile van v2.1.0: **24 bekende kwetsbaarheden in 6 pakketten** (authlib, certifi,
  dnspython, fastmcp, opentelemetry-api, pypdf). `cryptography` 50.0.0 is in orde.
- De controles (dagelijks `pip-audit`, wekelijkse review) staan buiten deze repo, in `~/Repos/stonosbv/gwmcp-watch/`.

## Nog te doen

### Onderhoud van de fork
- [ ] De geplande taken installeren: `~/Repos/stonosbv/gwmcp-watch/install.sh`.
- [ ] De 24 kwetsbaarheden beoordelen: per pakket de lockfile bijwerken op `stonos`
      (`uv lock --upgrade-package <pakket>`), of erkennen met een reden in `gwmcp-watch/acknowledged.txt`.
      Voorrang: **fastmcp** (GHSA-7xgj-8392-5q4r: autorisatie na transformatie wordt overgeslagen bij
      directe calls, mogelijk relevant voor verborgen tools), **certifi**, **authlib**.
      pypdf wordt alleen door de Docs- en Drive-tools gebruikt, niet met `--tools gmail`.
- [ ] De herkomst van `httpx2`, `httpcore2` en `fastmcp-slim` één keer nakijken.
- [ ] Altijd starten met `uv run --frozen`: `uv.lock` vermeldt een andere projectversie dan `pyproject.toml`.

### Gmail in de praktijk uitproberen
- [ ] Een Google Cloud-project met de Gmail API aan.
- [ ] Een service account met een JSON-sleutel (rechten 600, buiten repo's, Dropbox en iCloud).
- [ ] De Workspace-admin geeft domain-wide delegation met **alleen `gmail.readonly`**. Dat is de echte
      grens voor alleen-lezen: de server verbergt schrijftools, maar dwingt dat niet af op scope-niveau.
- [ ] De MCP toevoegen aan een testsessie met:
      `--read-only --tools gmail --disabled-tools start_google_auth`,
      plus `GOOGLE_SERVICE_ACCOUNT_KEY_FILE`, `USER_GOOGLE_EMAIL` en `DWD_ALLOWED_DOMAINS`.
- [ ] Testen: mail lezen werkt, en een schrijfactie wordt door Google geweigerd.
- [ ] Daarna: de sessies die nu van de claude.ai-connectoren afhangen hierop overzetten.

### Niet doen (uit de nulmeting)
- Geen `streamable-http` (bindt standaard op 0.0.0.0) en geen `WORKSPACE_MCP_HTTP_PORT`.
- Geen `ALLOWED_FILE_DIRS`, `OTEL_*`, `MCP_ENABLE_OAUTH21` of `TRUST_GATEWAY_IDENTITY`.
- Niet het niveau `gmail:organize`: dat vraagt `gmail.modify`, en daarmee kun je ook versturen.
