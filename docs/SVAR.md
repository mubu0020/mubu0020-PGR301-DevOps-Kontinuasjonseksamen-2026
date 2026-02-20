Oppgave 1 – DevOps-prosess for team (20 poeng) Branch-strategi
Main-branch skal alltid representere produksjonsklar kode. Det betyr at main til enhver tid skal være stabil, testet og klar til deploy. Ingen utvikling skal skje direkte på main. All ny funksjonalitet, feilretting og tekniske forbedringer skal utvikles i egne branches.
Teamet bruker følgende branch-typer:

•	feature/* for nye funksjoner

•	fix/* for bugfixer

•	chore/* for tekniske endringer (CI/CD, Docker, oppsett, etc.)

En ny branch opprettes hver gang man starter på en ny oppgave. Dette gjelder blant annet når man:

•	legger til tester

•	endrer Dockerfile

•	fikser en bug

•	lager en ny feature

•	endrer CI/CD

En enkel regel i teamet er at én branch skal brukes per oppgave. Dette gjør det enklere å holde oversikt og reduserer risikoen for at flere endringer blandes sammen.
Branchene navngis på en strukturert og beskrivende måte, for eksempel:

•	feature/add-unit-tests

•	feature/github-actions-ci

•	fix/docker-entrypoint

•	chore/trivy-scan

Dette er viktig når teamet vokser fra én til flere utviklere. Dårlige eller utydelige navn kan føre til misforståelser, mens gode navn er selvforklarende, enkle å spore og gir et ryddig og profesjonelt inntrykk.

Pull Request-prosess

En Pull Request (PR) skal opprettes når arbeidet i branchen er ferdig, koden fungerer lokalt og er klar for gjennomgang. I tillegg skal relevante tester være skrevet og bestått før PR opprettes.

Pull Requests fungerer som en kvalitetssikring før kode merges til main. Dette er spesielt viktig i et prosjekt hvor AI-verktøy kan brukes til å generere kode raskt. Selv om AI øker produktiviteten, kan det også introdusere dårlige løsninger, sårbarheter eller suboptimale konfigurasjoner. I et team på 3–5 utviklere bør minst én annen utvikler gjennomgå og godkjenne PR før merge. Ideelt sett kunne to godkjenninger vært ønskelig for ekstra kvalitetssikring, men i dette prosjektet er minimum én godkjenning tilstrekkelig.


Før en Pull Request kan merges må følgende være oppfylt:

•	Minst én godkjenning

•	Ingen merge conflicts

•	CI pipeline må være grønn

•	Ingen kritiske eller alvorlige sårbarheter Dette sikrer at main 
forblir stabil og at kvaliteten ikke avhenger av enkeltpersoner.

Branch Protection

For å sikre at reglene faktisk følges, er det konfigurert branch protection på main-branch.

Følgende regler er aktivert:

•	Require pull request before merging

•	Require at least 1 approving review

•	Block force pushes

•	Restrict deletions

•	Dismiss stale pull request approvals when new commits are pushed
Disse reglene hindrer direkte push til main og sørger for at all kode går gjennom Pull Request-prosessen. Dette er viktig når teamet vokser, da det skaper struktur og forutsigbarhet.

Dismiss stale pull request approvals when new commits are pushed er spesielt viktig fordi en tidligere godkjenning ikke skal gjelde dersom ny kode pushes etter review. Dette forhindrer at man ubevisst omgår kvalitetssikringen. Require status checks to pass vil bli aktivert etter at CI/CD-pipeline er etablert i Oppgave 3, siden GitHub krever at minst én status check eksisterer før regelen kan håndheves.

Automatisering

Automatisering er en sentral del av DevOps-prosessen. Når utviklingshastigheten øker, spesielt med bruk av AI-verktøy, må kvalitetssikringen også være automatisert.

Følgende automatiske sjekker skal kjøres:

På Pull Request:

•	mvn test

•	mvn package

•	Bygging av Docker image

•	Trivy filesystem scan

•	Trivy image scan

•	Pipeline skal feile ved HIGH eller CRITICAL sårbarheter

•	Generering av SARIF-rapport

Dette sikrer at kode kvalitetssikres før den merges til main.
På push til main:

•	mvn test

•	mvn package

•	Bygging av Docker image

•	Push av Docker image til Docker Hub

Ved å skille mellom PR-validering og main-build sikres det at kun verifisert og sikker kode blir publisert videre.
Automatiseringen gjør at kvalitet og sikkerhet ikke kun baseres på manuell kontroll, men håndheves konsekvent gjennom pipeline.

Dokumentasjon av konfigurasjon
Jeg har:

Opprettet en egen feature-branch for Oppgave 1 og pushet den til GitHub
Konfigurert branch protection på default branch (main)
Aktivert krav om Pull Request og minst én godkjenning før merge
Skjermbilder som viser opprettelse av branch og konfigurering av branch protection er inkludert under.
Opprettelse av sub-branch
![Opprettelse av sub-branch](/docs/images/Opprettelse-av-branch.png)

Konfigurering av ruleset i github !
![Konfigurering av ruleset i github](/docs/images/konfigurering-av-ruleset-github.png)

Resten av ruleset vindue
[Resten av ruleset vindue](/docs/images/ruleset-konfigurering-github.png)

Status-Cheks-ble lagt til etter oppgave3
![Status-Cheks-ble lagt til etter oppgave3](/docs/images/status-checks.png)

status-checks-etter-oppgave3-ruleset-github
![Status-checks etter oppgave3](/docs/images/status-checks-ruleset.png)

review required. Vi ser at rules for repo fungerer. ![review requierd](/docs/images/review-required.png)

standar for review
![alt text](/docs/images/review-standar.png)

Feature branches slettes etter merge for å holde repository ryddig og unngå at gamle  branches blir liggende: ![branch-slettes-etter-merge](/docs/images/branch-deletetion.png)

Oppgave 2

Slik de kan se på bildet har jeg opptimalisert Dockerfilen, og image som ble opprettet av Dockerfil før optimalisering heter quiz-app-old med disk usage 608oMB og det nye image av den optimaliserte dockerfilen heter quiz-app-new med disk usage 317MB. legger også ved bildet av den forbedrede Dockerfile
   
Jeg brukte Multi-stage build for å unngå at build-verktøy inkluderes i final image, noe som reduserer størrelsen og øker sikkerhet. Den opprinnelige Dockerfile brukte single-stage-build, noe som førte til at MAVEN og build-verktøy ble inkludert i final image. Det har blitt lagt med kommentarer i Dockerfilen, men en kort forklarelse I første stage brukes et Maven-image til å bygge JAR-filen. I andre stage brukes en minimal Java runtime (eclipse-temurin 21 JRE basert på Ubuntu Jammy), hvor kun den ferdigbygde JAR-filen kopieres inn. Dette reduserte image-størrelsen med bokstaveligtalt 50%. Ved skjermbildene nedenfor, kan du se størrelse på image før og etter optimalisering av Docker-fil. 

Image før optimalisering:
![Image før optimalisering](/docs/images/docker-image-old.png)

Image etter optimalisering: 
![docker image etter optimalisering](/docs/images/docker-image-new.png)

Testet lokalt som oppgaven ber om: ![test lokalt](/docs/images/test-lokalt.png)

Tester med curl: ![alt text](/docs/images/test-med-curl.png)

Selv om jeg visste at det ikke skulle fungere på nettleser, så testet jeg det og fikk 404 not found fordi applikasjonen kun eksponerer REST-endepunkt. Derfor fungerte kommando med curl til endepunkte /health. 

Forbedret Docker-fil: ![optimalized-docker-file](/docs/images/optimalized-docker-file.png)


Oppgave 3 

Jeg har faktisk gjort denne eksamensoppgaven 2 ganger nå, og valgte derfor å være strategisk. Jeg satt og feilsøkte i flere timer, og ble faktisk veldig flink vil jeg si. For i første omgang, kjørte jeg workflow uten security-lag (Trivy), for å se at workflowen fungerer: 

![Workflow-uten-security-lag](/docs/images/workflow-uten-security-lag.png)

På dette steget, har jeg lagt til Security-laget i workflowen, men av tidligere erfaringer. Passer jeg på at SARIF-rapport blir generert og lastet opp til GitHub-Security, for å faktisk få innsyn i mine vulnabilitiet, så skal pipeline feile. Derfor jeg valgte å sette exit-code:1 i førsteomgang, og ignore-unfixed: true. ![workflow-med-security-lag](/docs/images/workflow-med-security-lag.png)

Som forventet kan de se at den stoppet opp på filnivå. En annen ting jeg fant ut av var at Code scanning kun fungerer i main, så vi vil ikke få security-rapporten før merge av pull request med sub-branch, men med tanke sikkerhet og uten å forsikre meg om at vulnabilitiene er false-positive, kan jeg dessverre ikke deploye i main. En ting jeg faktisk valgte å gjøre, og igjen det kommer av at jeg har gjort denne oppgaven 2 ganger nå. Er å generere en tabell med vulnabiliteis med HIGH og CRITICAL, på denne måten, kan jeg deploye i subbranch og få en tilbakemelding om sårbarheter, uten å bryte med retningslinjer for dette prosjektet, som gjelder for teamet. Se retningslinjer (Oppgave.1) Aldri pushe usikker kode til main. 

Som Andre gang, så fikk jeg sårbarheiter og pipeline stoppet å kjøre: 
![workflow-security-fail](/docs/images/workflow-security-fail.png)
Dette var på grunn av sikkerhetskravene til Trivy. Fordi i pom.xml filen min bruker jeg, eldre versjon av Spring Boot, Tomcat, Spring Core og Netty, disse er også sårbare. Trivy krever nyere versjoner. Jeg endret depenciene som i pom.xml filene ved nyere versjon og ovveride av depencies.![override-dependecies](/docs/images/override-dependecies.png)

Forsikret meg om at jeg hadde riktige versjoner. ![Correct-versions-of-dep](/docs/images/correct-versions-of-dep.png)

Og fikk bort 15 HIGH og 1 CRITICAL sårbarhet/vulnabilitie. ![decreased-vulnabilities](/docs/images/decreased-vulnabilities.png)

Men det er 1 High igjen og det er Spring-Core. Det finnes en patched versjon, og Spring Boot kjører ikke den. Den kjører 6.2.6 men fixed version er 6.2.11. Override Spring-Core til patched version, og forsikret meg om at det var riktig versjon. 

![spring-core-correct-version](/docs/images/spring-core-correct-version.png)

Piplelinen stoppet igjen, men ikke på grunn av sårbarheter, men på grunn av at jeg lastet opp filesystemet SARIF og image SARIF, noe som gir kategori konflikt. Løste dette ved å sette unik ‘category’ på upload-stage. ![sarif-category](/docs/images/sarif-category.png)

Nedenfor har jeg lastet opp workflowen som kjører grønt på alt. Så nå har jeg en pipeline, som kjører unit-tester, bygger applikasjon, bygger container image, skanner filesystemet(Trivy), Skanner Docker-image(Trivy), Pipeline feiler ved HIGH/CRITICAL, SARIF lastes til Github Security tab. Merget med main, og la inn status checks på ruleset for main nå. Dette var ikke mulig u sub-branch på bakgrunn av pr-validate. 
![green workflow](/docs/images/workflow-complete.png)

Validate-pr kjører kun ved pull_request, og build and push kjører i main.  Dette ser vi etter merge med main. 
![all-checks-have-passed](/docs/images/all-checks-have-passed.png)

Nedenfor teksten kan du Code scanning rapporten som trivy lastet opp SARIF til GitHub Security Scanning: ![security-scanning-report](/docs/images/security-scanning-report.png)

lennke til vellykket workflow-kjøring på main: https://github.com/mubu0020/mubu0020-PGR301-DevOps-Kontinuasjonseksamen-2026/actions/runs/22204007561

Lenke til vellykket Pull Request-validering: https://github.com/mubu0020/mubu0020-PGR301-DevOps-Kontinuasjonseksamen-2026/actions/runs/22203881057


Oppgave 4. 

Denne oppgaven har jeg virkelig sett frem til. Som mange andre har jeg både et positivt og et mer skeptisk forhold til kunstig intelligens. I starten av dette prosjektet tenkte jeg at dersom jeg møtte utfordringer, kunne jeg støtte meg på AI-verktøy som ChatGPT, Microsoft Copilot og Claude. Det fungerte godt – helt til jeg kom til Oppgave 3, som var faktisk utfordrene men også veldig lærerikt.

Da Trivy security scan ble introdusert i pipelinen, oppstod det mange HIGH og CRITICAL vulnerabilities, særlig knyttet til dependencies i pom.xml. Jeg begynte å rette opp noen av dem, men feilene vedvarte. Etter flere runder med AI-assistanse fikk jeg anbefalinger om å ignorere filesystem-scanning og kun fokusere på image-nivå scanning. Begrunnelsen var at den ferdige containeren og den genererte JAR-filen var det som faktisk kjørte i produksjon, og at HIGH vulnerabilities på filnivå ofte var “false positives” i Spring Boot-prosjekter. AI foreslo også at jeg burde blokkere pipeline kun på CRITICAL, ikke HIGH.

Dette stod imidlertid i direkte konflikt med oppgaveteksten, som eksplisitt krevde at pipelinen skulle stoppe ved både HIGH og CRITICAL vulnerabilities. AI argumenterte videre med at det i praksis er vanlig å justere severity-nivå i bransjen, og at oppgaven ikke spesifiserte hvilket nivå stoppet skulle skje på. På dette tidspunktet forstod jeg at jeg ikke kunne stole blindt på anbefalingene. Dette ble et vendepunkt. AI hadde gjort meg mer effektiv, men den kunne ikke ta ansvaret for arkitektoniske og sikkerhetsmessige beslutninger. Jeg måtte selv verifisere dependency tree, inspisere JAR-filen i containeren og analysere CVE-ene manuelt. Først da kunne jeg fastslå hvilke sårbarheter som faktisk var reelle og hvilke som var false positives.

Denne erfaringen illustrerer hvorfor DevOps blir viktigere i en AI-akselerert verden. Når AI øker utviklingshastigheten, øker også risikoen for å introdusere feil, sårbarheter og dårlige praksiser raskere enn før. Uten robuste CI/CD-pipelines, automatiserte tester, sikkerhetsscanning og tydelige branch-protections, kan AI bidra til å spre feil i høyere tempo. DevOps handler ikke bare om automatisering, men om kontrollert automatisering. I dette prosjektet var det nettopp pipeline-reglene – exit-code på HIGH og CRITICAL, SARIF-rapportering og branch protection – som sikret kvalitet. AI kunne generere forslag, men det var DevOps-prinsippene som satte rammene. Jo mer produktive vi blir med AI, desto viktigere blir DevOps som sikkerhetsnett og kvalitetssystem. AI kan akselerere utvikling, men uten struktur, validering og sikkerhetsmekanismer kan den like raskt akselerere feil, som den gjorde med meg i dette prosjektet. Virker det som at Oppgave 3 gikk ganske lett for meg, men det var langt ifra. På grunn av missleding fra ChatGBT ble jeg faktisk mer kompetent innenfor fagfeltet og hadde ikke noe valg annet enn det, for jeg var nødt til å validere the ChatGBT foreslo  
