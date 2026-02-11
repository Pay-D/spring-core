# spring-core

Build Platform für Spring Boot Projekte.

Dieses Repository stellt ein eigenes Parent-POM bereit, 
das in anderen Projekten als `<parent>` verwendet wird und `spring-boot-starter-parent` ersetzt.

---

# Ziel

`sprint-core` dient als zentrale Build- und Governance-Plattform für alle Spring-Projekte.

Es ermöglicht:

* Zentrale Versionierung aller Dependencies
* Einheitliche Plugin-Konfiguration
* Code-Quality Enforcement
* Standardisierte Test-Infrastruktur
* Konsistente Logging- und OpenAPI-Konfiguration
* CI/CD Standardisierung
* Einfaches Upgrade durch Parent-Version-Update

Neue Services müssen nur:

```xml
<parent>
    <groupId>de.payd.spring.core</groupId>
    <artifactId>spring-core-parent</artifactId>
    <version>1.0.0</version>
</parent>
```

setzen — und sind sofort vollständig konfiguriert.

---

# Architektur

Repository-Struktur:

```
spring-core/
├── pom.xml                         (Aggregator)
├── spring-core-dependencies/       (BOM)
│   └── pom.xml
└── spring-core-parent/             (Corporate Parent)
    └── pom.xml
```
---

## 1. Aggregator (Root)

Zweck:

* Baut alle Module
* Definiert gemeinsame Version
* Enthält keine Dependencies
* Enthält keine Plugins
---

## 2. spring-core-dependencies (Corporate BOM)

Zweck:
* Importiert Spring Boot BOM
* Definiert alle Versions zentral
* Kontrolliert Drittanbieter-Bibliotheken
* Verhindert Version Drift
* Garantiert Dependency Alignment

### Enthalten:

* Spring Boot BOM
* MapStruct
* Lombok
* Testcontainers BOM
* springdoc OpenAPI
* Logging Stack
* Test Frameworks

### Prinzip:

Child-Projekte dürfen KEINE Versionen definieren.

---

## 3. spring-core-parent (Corporate Parent)

Zweck:

* Ersetzt `spring-boot-starter-parent`
* Importiert `spring-core-dependencies`
* Definiert Plugin Management
* Erzwingt Java 25
* Erzwingt Maven Version
* Aktiviert Quality Tools
* Standardisiert Build Lifecycle

---

# Warum eigenes Parent statt spring-boot-starter-parent?

Spring Boot Parent:

* Versteckt Plugin-Versionen
* Limitiert Governance
* Keine zentrale Erweiterung möglich
* Keine Corporate Policies

spring-core:

* Volle Plugin-Kontrolle
* Volle Dependency-Kontrolle
* Einheitliche Code Quality
* Unternehmensweite Standards
* Einfache Upgrades durch Parent-Version

---

# 📦 Dependency Management

Spring Boot wird über BOM importiert:

```
spring-boot-dependencies
```

Zusätzlich gemanagt:

* MapStruct
* Lombok
* Testcontainers
* OpenAPI (springdoc)
* Logging
* Test Frameworks

Prinzip:

→ Child-Projekte geben KEINE Versionen an.

---

# 🔧 Plugin Management

Zentral verwaltet im Parent:

* maven-compiler-plugin
* maven-surefire-plugin
* maven-failsafe-plugin
* spring-boot-maven-plugin
* maven-enforcer-plugin
* spotless-maven-plugin
* maven-checkstyle-plugin
* jacoco-maven-plugin

Child-Projekte dürfen keine Plugin-Versionen definieren.

---

# 🧹 Code Quality & Clean Code Enforcement

## Spotless

* Google Java Format
* Build schlägt fehl bei Formatabweichung
* Optional: `spotless:apply`

## Checkstyle

* Corporate Coding Standard
* Zentrale checkstyle.xml
* Naming, Package, Structure Enforcement

## Jacoco

* Coverage Reports
* Sonar Integration
* verify-Phase

## Enforcer

Erzwingt:

* Java Version
* Maven Version
* Dependency Convergence

---

# 🧪 Test Infrastruktur

Testcontainers BOM integriert.

Services können einfach nutzen:

```xml
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>postgresql</artifactId>
    <scope>test</scope>
</dependency>
```

Ohne Version.

---

# 📖 OpenAPI Standard

Vorkonfiguriert:

```
springdoc-openapi-starter-webmvc-ui
```

Swagger UI erreichbar unter:

```
/swagger-ui.html
```

---

# 🪵 Logging

Standardisiert auf:

* SLF4J
* Logback
* Spring Boot Logging

Erweiterbar um:

* JSON Logging
* Correlation IDs
* MDC Standard
* Strukturierte Logs

---

# 🔁 Reproducible Builds

Aktiviert:

* Deterministische Artefakte
* Output Timestamp
* CI reproduzierbar

---

# 🚀 Verwendung in Service-Projekten

In einem neuen Service:

```xml
<parent>
    <groupId>de.payd.spring.core</groupId>
    <artifactId>spring-core-parent</artifactId>
    <version>1.0.0</version>
</parent>
```

Danach können Dependencies ohne Version genutzt werden:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Upgrade eines Services:

Nur Parent-Version erhöhen.

---

# 🔄 CI/CD Integration

Empfohlen:

* GitHub Actions
* Maven Wrapper
* mvn verify
* Spotless Check
* Checkstyle
* Tests
* Jacoco
* Sonar Scan

Build schlägt fehl bei:

* Format-Verstoß
* Checkstyle Fehler
* Testfehler
* Dependency-Konflikten

---

# 📈 Versionierungsstrategie

Alle Module haben gleiche Version:

```
1.0.0
```

Release-Prozess:

1. Version im Root erhöhen
2. `mvn clean install`
3. Publish in internes Artifact Repository
4. Services ziehen neue Parent-Version

---

# 🏢 Ziel-Einsatz

Geeignet für:

* Microservice-Landschaften
* Multi-Repo Organisationen
* Corporate Governance
* Standardisierte DevOps Pipelines
* Entwicklerplattform-Strategien

---

# 🔐 Governance Regeln

Service-Projekte dürfen:

* Keine Plugin-Versionen definieren
* Keine Dependency-Versionen definieren
* Keine Java Version ändern
* Keine Compiler-Konfiguration überschreiben

spring-core ist die einzige Quelle der Wahrheit.

---

# IDE Hinweise

* Annotation Processing aktivieren
* Lombok Plugin installieren
* Maven Auto-Import aktivieren

---

# 🧠 Fazit

`sprint-core` ist keine Template-App.

Es ist eine Build-Plattform.

Es ersetzt bewusst `spring-boot-starter-parent`, um:

* Version Drift zu verhindern
* Code Qualität zu erzwingen
* Wartung zu zentralisieren
* Upgrades zu vereinfachen

Ein Versions-Upgrade im Parent wirkt sich kontrolliert auf alle Services aus.

---

Wenn du willst, machen wir als nächsten Schritt:

1. Exakte Modulnamen finalisieren
2. Saubere Artefakt-Namen definieren
3. Dann bauen wir die finale Root + BOM + Parent POMs sauber auf

Jetzt sind wir architektonisch 100% aligned.
