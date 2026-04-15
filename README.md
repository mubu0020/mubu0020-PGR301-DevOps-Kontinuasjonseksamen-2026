# 🚀 PGR301 DevOps – Kontinuasjonseksamen 2026

![CI](https://img.shields.io/badge/CI-GitHub_Actions-blue)
![Build](https://img.shields.io/badge/build-passing-brightgreen)
![Security](https://img.shields.io/badge/security-scanned-success)
![Docker](https://img.shields.io/badge/container-Docker-blue)

Dette repositoryet inneholder min besvarelse til kontinuasjonseksamen i **PGR301 DevOps**.

Løsningen demonstrerer en moderne DevOps-pipeline med fokus på:
- Automatisering
- Sikkerhet (multi-layer scanning)
- Effektive workflows
- Reproduserbar deploy

---

## 🧭 Om prosjektet

Applikasjonen er en enkel **Quiz API** utviklet med Spring Boot.

Den er bevisst holdt enkel for å flytte fokus til:
- CI/CD
- Sikkerhet
- Infrastruktur og leveranseprosess

---

## 🏗️ Overordnet arkitektur


Developer → GitHub → GitHub Actions → Build/Test → Security Scan → Docker Build → Image Scan → Resultater i GitHub Security


Pipeline fungerer som en komplett kvalitetssikring før kode kan merges eller deployes.

---

## ⚙️ CI/CD Pipeline

Pipeline er implementert med **GitHub Actions** og trigges ved:
- Push
- Pull Request

### 🔁 Pipeline steg

1. **Build**
   - Maven build av applikasjonen

2. **Test**
   - Kjører enhetstester
   - Stopper pipeline ved feil (fail fast)

3. **File-level security scanning**
   - Scanner kildekode og dependencies
   - Oppdager kjente sårbarheter tidlig

4. **Docker build**
   - Bygger container image

5. **Image scanning (OSC)**
   - Scanner container image for sårbarheter
   - Dekker OS packages og layers

6. **SARIF reporting**
   - Resultater sendes til GitHub Security-tab

---

## ⚡ Workflow-optimalisering

Pipeline er aktivt forbedret for ytelse og kvalitet:

- **Fail fast** → stopper tidlig ved feil
- **Redusert redundans** → ingen unødvendige steg
- **Tydelig separasjon av ansvar**
- **Effektiv rekkefølge på jobs**
- **Bedre lesbarhet og vedlikeholdbarhet**

Dette gir:
- Raskere feedback
- Mindre ressursbruk
- Enklere debugging

---

## 🔐 Sikkerhet (kjerne i løsningen)

Sikkerhet er implementert i flere lag:

### 📄 File-level scanning
- Scanner kode og dependencies
- Oppdager sårbarheter før build fullføres
- Implementerer *shift-left security*

### 📦 Dependency scanning
- Identifiserer kjente CVE-er
- Bidrar til tryggere dependency-håndtering

### 🐳 Image scanning (OSC)
- Scanner Docker image etter build
- Avdekker sårbarheter i OS og libraries
- Viktig for produksjonsmiljø

### 📊 SARIF (GitHub Security)
- Resultater vises direkte i GitHub
- Gir:
  - Oversikt
  - Sporbarhet
  - Enkel oppfølging

---

## 🐳 Docker

Applikasjonen er containerisert for konsistent kjøring.

### Bygg og kjør

```bash
docker build -t quiz-app .
docker run -p 8080:8080 quiz-app
Hvorfor Docker?
Samme miljø overalt
Enklere deploy
Mindre "it works on my machine"-problemer
🔌 API
Endpoint
POST /quiz
Request
{
  "answers": ["A", "B", "C"]
}
Response
{
  "score": 2
}
🧪 Testing
Enhetstester kjøres automatisk i CI
Pipeline feiler hvis tester feiler
Gir rask feedback og høyere kvalitet

Kjør lokalt:

mvn test
🔒 GitHub governance

Repoet bruker moderne GitHub-praksis:

Branch protection
Required status checks
Pull request-basert utvikling

Dette sikrer:

Stabil main branch
Kvalitetskontroll
Sporbar historikk
📁 Struktur
.
├── src/
├── .github/workflows/
├── Dockerfile
├── pom.xml
└── docs/
🛠️ Kjør lokalt
mvn clean install
mvn spring-boot:run

Applikasjonen kjører på:

http://localhost:8080
🧠 DevOps-refleksjon

Denne løsningen fokuserer på hvordan software leveres, ikke bare hva den gjør.

Viktige prinsipper demonstrert:

🔁 Kontinuerlig integrasjon
⚡ Automatisering av hele pipeline
🔐 Shift-left security
📦 Reproduserbarhet med Docker
👀 Synlighet gjennom GitHub Security

Pipeline fungerer som en kvalitetssikring som automatisk verifiserer:

Funksjonalitet
Sikkerhet
Byggbarhet
🏁 Oppsummering

Prosjektet demonstrerer en komplett DevOps-tilnærming med:

Effektiv CI/CD pipeline
Sikkerhet på flere nivåer
Optimalisert workflow
Container-basert deploy

Fokuset har vært på å bygge en robust og moderne leveranseprosess.
