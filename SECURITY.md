# Security Policy

This policy covers **IceBox Engine** and everything shipped with it: the editor,
the launcher, the updater, the runtime that goes into a built game, the prebuilt
core libraries, the build tooling and the bundled sample content.

IceBox Engine is proprietary software published by IceBoxCrew Studio. It is
distributed as an installer, not as source, so the source tree is not public and
you are not expected to have read it in order to report something. A clear
description of what you observed is enough.

---

## Reporting a vulnerability

**Please do not open a public issue for a security problem.** Use one of these
instead:

1. **Email — preferred:** [iceboxcrew057@gmail.com](mailto:iceboxcrew057@gmail.com),
   with `SECURITY` in the subject line.
2. **GitHub private vulnerability reporting:**
   [open a private advisory](https://github.com/IceBoxCrew/IceBoxEngine/security/advisories/new).

If you would rather encrypt the report, say so in a first message with no
details in it and we will arrange a key.

### What to include

The more of this you can give, the faster it gets fixed — but send the report
even if you only have some of it.

- What happens, and why you believe it is a security problem rather than a bug.
- Engine version (shown in the launcher's **About** tab), operating system and
  CPU architecture, and whether it was a Release or Debug build.
- Which component: **Editor**, **Launcher**, **Updater**, **Runtime / a built
  game**, the **build tooling**, or the **licensing and activation** system.
- Steps to reproduce. A minimal project, script or asset file that triggers it is
  worth more than anything else you can send.
- Logs, a stack trace, a crash report file, or a proof of concept, if you have
  them.
- How you would like to be credited, or that you would rather not be.

### Please do not

- Test against anyone else's machine, project or game without their permission.
- Access, modify, exfiltrate or retain data that is not yours.
- Run denial-of-service, spam or load tests against our activation, update or
  crash-report endpoints.
- Disclose the issue publicly before we have had a reasonable chance to fix it —
  see **Coordinated disclosure** below.

---

## What we commit to

IceBoxCrew Studio is a very small studio, so these are honest targets rather than
a contractual service level:

| Stage | Target |
| ----- | ------ |
| Acknowledge that your report arrived | within **3 working days** |
| Tell you whether we consider it a vulnerability, and how severe | within **10 working days** |
| Keep you updated while we work on it | at least every **14 days** |
| Ship a fix for a critical issue | as fast as we can build and publish a release on every platform |

If you have not heard from us within the acknowledgement window, please send a
reminder — a missed report is far more likely than a deliberate silence.

## Safe harbour

If you make a good-faith effort to follow this policy, we will treat your
research as authorised. We will not pursue or support legal action against you
over it, and we will not treat it as a breach of the IceBox Engine License
Agreement — including its restriction on reverse engineering, which does not
apply to security research carried out under this policy.

This protection covers your research. It does not cover using a finding to harm
users, to access other people's data, or to defeat the license check for your own
benefit or anyone else's.

## Coordinated disclosure

We ask for **90 days** from your first report before public disclosure, and we
will usually be finished well before that. If a fix is going to take longer, we
will tell you why and agree a date with you rather than let the clock run out in
silence. If an issue is already being exploited, we will move immediately and
coordinate the announcement with you.

Once a fix ships, we publish the advisory ourselves and credit you by the name or
handle you asked for, unless you asked for anonymity.

---

## Supported versions

Security fixes go into **the current release of IceBox Engine**, and that release
is delivered to everyone through IceBox Updater.

Every update is free for every licence holder, forever, across major versions —
that is a term of the licence, not a promotion. There is therefore no such thing
as an older edition that has to keep receiving backports: updating to the current
version is always available to you, always free, and is how you receive a
security fix.

If you are running an older version and cannot update for some reason, write to
us and say why. We would rather know.

---

## Scope

### In scope

| Area | Examples |
| ---- | -------- |
| **Editor, launcher, updater** | Memory-safety bugs reachable from a project, an asset or a scene file; arbitrary code execution from opening a crafted project; privilege escalation |
| **Asset and project parsing** | Crafted textures, fonts, tilemaps, animations, audio, video, level files or `.ice*` assets that corrupt memory or execute code |
| **Runtime and built games** | Anything that lets a crafted save file, a downloaded asset, or network traffic compromise a player's machine |
| **Updater** | Man-in-the-middle on the update path, checksum or version-check bypass, an update that installs without the user's confirmation, path traversal while unpacking |
| **Licensing and activation** | Forging or replaying an activation response, extracting signing material from a shipped build, leaking a key or a Device ID to a third party |
| **Crash reporting** | Anything that causes more to be transmitted than `PRIVACY_NOTICE.txt` describes, or that transmits without the user pressing Send |
| **Networking** | ENet and WebSocket transport, the transport encryption, and anything reachable by a remote peer of a game |
| **Build tooling** | Command or script injection through project names, paths or build settings; a produced build that contains material it should not |
| **Installers** | Privilege escalation during install or uninstall, unsafe paths, unsafe permissions on installed files |
| **Third-party components we ship** | A known-vulnerable version of a bundled library reaching your machine through our installer is **our** problem to fix, even though the bug is upstream. Please report it |

### Out of scope

- Bugs with no security consequence — crashes that are simply crashes, rendering
  glitches, performance problems. Those are ordinary
  [issues](https://github.com/IceBoxCrew/IceBoxEngine/issues) and are very welcome
  there.
- Attacks that require an attacker to already have physical access to the machine,
  or an account on it with administrator rights.
- Social engineering of us or of our users.
- Missing hardening that is not exploitable on its own (a compiler flag, an
  absent mitigation) — still worth telling us about by email, but it will not be
  treated as a vulnerability.
- Anything in software we do not ship: your own game code, a third-party plugin
  you installed, or Ollama and the models it runs for the AI Helper plugin.
- Reports produced by a scanner with no analysis attached and no demonstrated
  impact.

---

## How the product behaves, so you know what to expect

These are deliberate design decisions. If you find the software behaving
otherwise, that itself is a report worth sending.

- **A built game contains no licensing code.** The license check is compiled into
  the editor, the launcher and the updater only. A game you ship contains no key,
  no device identifier and no activation code, and never contacts us.
- **There is no telemetry.** The editor, launcher and updater transmit exactly two
  things, and only when you act: a crash report, if you press Send, and the single
  license activation request. The updater additionally asks a release manifest
  whether a newer version exists, which says nothing about you and can be switched
  off. `PRIVACY_NOTICE.txt` is the full description.
- **Updates are verified and never silent.** The updater fetches over HTTPS with
  certificate and hostname verification enabled, checks the downloaded file
  against the SHA-256 and the size published in the manifest, and installs only
  after you confirm. If a manifest entry carries no usable checksum, the updater
  says so rather than pretending the download was verified.
- **License keys are signed.** A key is verified locally against a public key
  compiled into the build. Activation happens once; after it succeeds the software
  never checks again, and it keeps working if our service disappears.
- **Attribution travels with your game.** `THIRD_PARTY_NOTICES.txt` and
  `ThirdPartyLicenses/` are placed in every build on every platform.

---

## Trust boundaries you should know about

Being straight about these is more useful than implying protection that is not
there.

- **A project is executable code.** Opening someone else's IceBox project runs
  their Lua and Python scripts with your user's privileges, exactly as opening a
  project in any other engine does. Treat a project from a stranger the way you
  would treat a program from a stranger. A crafted project that escapes *your own*
  expectations — for instance by exploiting a parser bug rather than by simply
  running a script — is in scope and we want to hear about it.
- **Mods are isolated by namespace, not by capability.** Each mod script runs in
  its own Lua environment so that mods do not collide with each other or with the
  game. That is an isolation mechanism, not a security sandbox: a mod is not
  prevented from doing what Lua can do. If you enable mod support in your game,
  you are choosing to run code your players supply, and that is your decision to
  make and to communicate to them.
- **Native plugins are native code.** A C++ plugin built against the SDK runs
  inside the editor or the game with full privileges. There is no boundary around
  it and there cannot be one.
- **Python in the editor is a tooling language.** Editor Python scripts automate
  the editor and have the access the editor has. They are not exposed to players
  and are not part of a shipped game.
- **The Web3 API talks to third-party services.** A Web build made with the Web3
  option contacts public RPC endpoints and an IPFS gateway from the player's
  browser, and interacts with whatever wallet the player has installed. Those are
  third parties with their own terms and their own privacy practices. Point them
  at endpoints you control before you ship anything real.

---

## Advisories

Fixed issues are announced in the release notes of the version that carries the
fix, and, where they matter to people who have already shipped a game, as a
[security advisory](https://github.com/IceBoxCrew/IceBoxEngine/security/advisories)
on this repository.

---

Thank you for taking the time to report something rather than ignoring it. It is
genuinely appreciated.

*IceBoxCrew Studio — [iceboxcrew057@gmail.com](mailto:iceboxcrew057@gmail.com) — [ice-box-crew.com](https://www.ice-box-crew.com/)*
