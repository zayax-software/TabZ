# TabZ — Schutz, Lizenz, Server

## Testphase (aktuell)

- **TabZ ist kostenlos** (`licensePhase=free-test` im Release-Build).
- Es gibt **keine** Abo-Abfrage und **keinen** eigenen TabZ-Server.
- Release-ZIPs (`build-closed-test.bat` setzt `-PtabzReleaseBuild=true`) aktivieren **lokale Integritätsprüfungen**:
  - signiertes Plugin-Zertifikat (SHA-256 aus `certificate/chain.crt`, wenn vorhanden), **oder**
  - typischer JetBrains-Plugin-Pfad (`…/JetBrains/…/plugins/…`).
- Unsignierte Kopien aus Git (`./gradlew runIde` / selbst gebautes ZIP ohne Release-Flag) sind **Development-Builds** und bleiben uneingeschränkt nutzbar.

## Brauchst du ein Backend für Lizenzen?

| Modell | Eigener Server? |
|--------|------------------|
| **JetBrains Marketplace — Paid Plugin / Subscription** | **Nein.** JetBrains prüft Kauf und Laufzeit über Marketplace-Konto und deren Infrastruktur. Integration erfolgt über die [Paid Plugins API](https://plugins.jetbrains.com/docs/marketplace/paid-plugins.html) im Plugin. |
| **Eigenes Abo (Keys, Stripe, Revocation, Offline-Limits)** | **Ja** (oder du akzeptierst, dass sideloaded/self-built Builds die Prüfung umgehen können). |
| **Nur EULA + Signing + offizielle Distribution** | **Nein** — schützt vor Casual-Klonen, nicht vor motiviertem Reverse Engineering (Quellcode öffentlich). |

## Was im Code passiert

- `TabzLicenseGate` — Testphase: frei; blockiert nur **Release-Builds** ohne vertrauenswürdige Signatur/Installationskontext (mit einmaliger Session-Ausnahme in der IDE).
- `TabzInternalTools` — Perf-/Demo-/Diagnose-Harnesses nur in **Development**-Builds (Release: nur mit `-Dtabz.internalTools=true`).
- `TabzPluginIntegrity` — rein lokal, **kein Netzwerk**.

## Später bezahlen (ohne eigenen Server)

Empfohlen: **JetBrains Paid Plugin** aktivieren, EULA anpassen, `TabzLicenseGate` an die Marketplace-Lizenz-API anbinden. Kein TabZ-Backend nötig.

## Was Code allein nicht verhindert

- Öffentliches Repo → jeder kann einen **Development-Build** bauen.
- Harte technische Sperre ohne Server ist bei Client-only-Plugins begrenzt; Kombination aus **Marketplace**, **Signing**, **geschlossenem Repo** (optional) und **Recht (EULA)** ist üblich.
