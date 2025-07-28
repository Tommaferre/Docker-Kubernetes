# Kubernetes vs Docker Compose - Guida Tecnica

## Panoramica

**Kubernetes** è un orchestratore di container enterprise per ambienti distribuiti multi-nodo, mentre **Docker Compose** è uno strumento per definire e gestire applicazioni multi-container su singoli host.

## Features Principali di Kubernetes

### 1. Pod e Gestione Container

I **Pod** sono l'unità base di deployment in Kubernetes e possono contenere uno o più container che condividono storage e rete. Ad esempio, un pod può includere un container nginx e un sidecar per log collection che operano insieme.

**Vantaggio K8s:** Container strettamente accoppiati condividono risorse automaticamente  
**Compose:** Ogni servizio è isolato, comunicazione via rete Docker

### 2. Deployments e Replica Management

I **Deployments** gestiscono automaticamente il numero di repliche desiderate. Se specifichi 3 repliche per un'applicazione web, Kubernetes garantisce che ci siano sempre 3 istanze attive, sostituendo automaticamente quelle che falliscono.

**Vantaggio K8s:** Self-healing, rolling updates senza downtime, rollback automatici  
**Compose:** Scaling manuale, restart manuale in caso di failure

### 3. Service Discovery e Load Balancing

I **Services** forniscono un endpoint stabile per raggiungere i pod. Un service "web-frontend" distribuisce automaticamente il traffico tra tutte le repliche disponibili, anche quando i pod vengono ricreati con IP diversi.

**Vantaggio K8s:** Load balancing nativo, service discovery DNS automatico  
**Compose:** Richiede configurazione manuale di proxy esterni

### 4. Gestione Configurazione

**ConfigMaps** e **Secrets** separano la configurazione dal codice. Una ConfigMap può contenere parametri di database, mentre i Secrets gestiscono password crittografate, entrambi iniettabili nei container senza rebuild delle immagini.

**Vantaggio K8s:** Gestione centralizzata, encryption nativa, rotazione automatica  
**Compose:** File env e secrets basic, no encryption integrata

### 5. Storage Persistente

I **Persistent Volumes** forniscono storage che sopravvive ai pod. Un database PostgreSQL può utilizzare storage distribuito che rimane disponibile anche se il pod viene ricreato su un nodo diverso.

**Vantaggio K8s:** Storage dinamico, replica cross-node, backup automatici  
**Compose:** Volume locali vincolati al singolo host

### 6. Ingress e Routing

Gli **Ingress Controllers** gestiscono il traffico HTTP/HTTPS esterno. Possono instradare `api.example.com` ai servizi backend e `app.example.com` al frontend, con SSL termination automatica.

**Vantaggio K8s:** Routing avanzato, SSL automatico, rate limiting  
**Compose:** Richiede reverse proxy separato

### 7. Health Checks e Monitoring

Kubernetes implementa **liveness** e **readiness probes**. Se un container non risponde agli health check, viene automaticamente riavviato. I readiness probe evitano di inviare traffico a pod non ancora pronti.

**Vantaggio K8s:** Health monitoring integrato, auto-recovery  
**Compose:** Health check basic, no auto-recovery avanzato

## Confronto Pratico

### Scenario 1: Sviluppo Locale

**Docker Compose** è superiore per semplicità di setup, debug immediato e resource footprint ridotto. Ideale per team piccoli o prototipazione rapida.

### Scenario 2: Ambiente di Produzione

**Kubernetes** eccelle in high availability, scalabilità automatica e gestione di failure complessi. Essenziale per applicazioni critiche multi-team.

### Scenario 3: Microservizi Complessi

**Kubernetes** offre service mesh integration, distributed tracing e gestione avanzata delle dipendenze tra servizi.

## Pro e Contro di Kubernetes

### Vantaggi

- Scalabilità automatica: Horizontal Pod Autoscaler adatta automaticamente le repliche in base a CPU/memoria
- Self-healing: Riavvio automatico di container falliti e sostituzione di nodi unhealthy
- Rolling updates: Deploy senza downtime con rollback automatico in caso di problemi
- Multi-cloud portability: Stessa configurazione funziona su AWS, GCP, Azure, on-premise
- Ecosystem ricco: Helm charts, Operators, service mesh (Istio), monitoring (Prometheus)
- Resource management: Limiti e richieste di CPU/memoria per ottimizzazione delle risorse
- Network policies: Controllo granulare del traffico tra servizi per sicurezza
- Storage dinamico: Provisioning automatico di volumi persistenti
- Service discovery nativo: DNS interno per comunicazione tra servizi
- RBAC integrato: Controllo accessi fine-grained per utenti e servizi

### Svantaggi

- Complessità estrema: Curva di apprendimento molto ripida, concetti astratti difficili
- Overhead operazionale: Richiede team dedicato per setup, monitoring, troubleshooting
- Resource intensive: Cluster minimo richiede 2-4GB RAM solo per i componenti di sistema
- Debugging complesso: Log distribuiti, networking overlay, troubleshooting multi-layer
- Configurazione verbosa: YAML complessi, facili errori di indentazione e sintassi
- Vendor lock-in nascosto: Dipendenza da cloud provider per load balancer, storage
- Costi elevati: Managed services costosi, nodi sempre attivi anche per carichi bassi
- Security complexity: Superficie di attacco ampia, configurazioni security complesse
- Breaking changes frequenti: API deprecation, necessità aggiornamenti frequenti
- Latenza networking: Overhead di proxy, service mesh, multiple network hops

### Svantaggi Docker Compose

- Single-host only: No distribuzione multi-nodo, single point of failure
- Scaling limitato: Solo vertical scaling, no load balancing automatico
- No self-healing: Container falliti richiedono intervento manuale
- Gestione secrets primitiva: File-based, no encryption, no rotazione automatica
- Network isolation basic: Limited network policies e security controls
- Storage locale: Volume bound al singolo host, no replica o backup automatici
- No service discovery avanzato: Solo network aliases basic
- Monitoring limitato: No metriche native, log centralization manuale

## Quando Usare Cosa

**Usa Docker Compose quando:**

- Sviluppo locale o testing
- Applicazioni semplici single-host
- Team piccoli senza expertise Kubernetes
- Budget limitato

**Usa Kubernetes quando:**

- Applicazioni production-critical
- Necessità di scalabilità automatica
- Architetture microservizi complesse
- Team dedicati DevOps/Platform
