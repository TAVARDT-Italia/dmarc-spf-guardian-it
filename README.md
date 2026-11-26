<p align="center">
  <a href="https://ag.tavardt.com/it/">
    <img src="https://ag.tavardt.com/img/tavardt-banner.png" alt="TAVARDT" width="1024" />
  </a>
</p>

# Guardiano DMARC, SPF e DKIM

[![Audit Tecnico](https://img.shields.io/badge/TAVARDT-Sicurezza%20Email-5b3cf5.svg)](https://tools.tavardt.com/it/verifica-dmarc-email/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Licencia: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Auditoria dei record DNS di posta per la prevenzione di spoofing, phishing e conformità tecnica con le linee guida ACN e il GDPR.

Ingegnerizzato e gestito dalla divisione di ciberdifesa di **TAVARDT** per le organizzazioni italiane.

***

## 1. Funzionalità e Capacità di Verifica

Questa utilità da terminale analizza i record DNS di autenticazione, la postura anti-spoofing e le chiavi crittografiche per organizzazioni in Italia:

* **Applicazione della Policy DMARC:** Ispezione approfondita delle direttive di imposizione (reject, quarantine e none), policy di sottodominio (sp) e percentuale (pct).
* **Auditoria del Record SPF:** Verifica dei meccanismi di mittenti autorizzati, limiti di query DNS e qualificatori rigorosi (-all vs ~all).
* **Verifica della Firma DKIM:** Validazione crittografica della chiave pubblica sui selettori DNS configurati.
* **Telemetria Aggregata e Forense:** Verifica dei circuiti di invio report aggregati (rua) per rilevare incidenti di Business Email Compromise.
* **Zero Dipendenze Esterne:** Motore nativo di DNS-over-HTTPS (DoH) con fallback automatico su comando nslookup di sistema.

***

## 2. Baseline di Autenticazione e Matrice di Valutazione

<table>
  <thead>
    <tr>
      <th>Controllo di Autenticazione</th>
      <th>Query DNS di Destinazione</th>
      <th>Baseline Consigliata</th>
      <th>Peso nel Punteggio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Politica DMARC (p)</strong></td>
      <td><code>_dmarc.&lt;dominio&gt;</code></td>
      <td><code>p=reject</code> (Scarto immediato di email non autorizzate)</td>
      <td>45 punti</td>
    </tr>
    <tr>
      <td><strong>Report Aggregati DMARC (rua)</strong></td>
      <td><code>_dmarc.&lt;dominio&gt;</code></td>
      <td>URI <code>mailto:</code> valida per telemetria aggregata in XML</td>
      <td>10 punti</td>
    </tr>
    <tr>
      <td><strong>Qualificatore Rigoroso SPF</strong></td>
      <td><code>&lt;dominio&gt;</code></td>
      <td><code>-all</code> (Blocco rigido per server mittenti non autorizzati)</td>
      <td>30 punti</td>
    </tr>
    <tr>
      <td><strong>Chiave Pubblica DKIM</strong></td>
      <td><code>&lt;selettore&gt;._domainkey.&lt;dominio&gt;</code></td>
      <td>Record pubblico valido (<code>v=DKIM1; p=...</code>)</td>
      <td>15 punti</td>
    </tr>
    <tr>
      <td><strong>Soglia di Conformità</strong></td>
      <td>Valutazione Complessiva</td>
      <td>Punteggio &gt;= 85 (Conforme - Difesa Rigorosa Applicata)</td>
      <td>100 punti Totali</td>
    </tr>
  </tbody>
</table>

***

## 3. Installazione e Avvio Rapido

Richiede Python 3.8 o superiore. Sviluppato esclusivamente con la libreria standard di Python, senza dipendenze esterne.

```bash
git clone https://github.com/TAVARDT-Italia/dmarc-spf-guardian-it.git
cd dmarc-spf-guardian-it
python guardian.py esempio.it
```

Esegui lo strumento direttamente tramite Python senza necessità di installare pacchetti terzi:

```bash
python guardian.py esempio.it -s default -j
```

* **Risoluzione DoH Nativa:** Invia query sicure DNS-over-HTTPS direttamente a Cloudflare, prevenendo avvelenamenti del DNS locale.
* **Fallback Automatico:** Passa senza interruzioni al comando `nslookup` nativo del sistema se le connessioni HTTPS in uscita sono bloccate.
* **Output Strutturato in JSON:** Supporta l'integrazione immediata in pipeline di monitoraggio continuo e verifica tramite `-j`.

***

## 4. Domande Frequenti (FAQ Tecnico)

### 1. Perché la politica DMARC p=reject è preferibile rispetto a p=none o p=quarantine?
La direttiva p=none opera esclusivamente in modalità monitoraggio, consentendo il recapito di email contraffatte. La transizione a p=quarantine sposta i messaggi non autorizzati nella cartella spam, mentre p=reject ne impone lo scarto immediato, bloccando il phishing mirato.

### 2. Quale rischio comporta una configurazione SPF con qualificatori +all o ?all?
Il qualificatore +all autorizza espressamente qualsiasi host presente su Internet a inviare email per conto del dominio. Il qualificatore ?all indica neutralità, annullando di fatto la protezione e lasciando la gestione del rischio al server ricevente.

### 3. In che modo guardian.py interroga i record DNS senza librerie esterne?
Lo strumento invia richieste DNS-over-HTTPS in formato JSON all'endpoint cloudflare-dns.com sfruttando le librerie standard di Python. In presenza di filtri di rete, il programma ripiega automaticamente sull'esecuzione del comando di sistema nslookup.

### 4. Qual è la rilevanza del parametro rua nella configurazione DMARC?
Il parametro rua specifica le caselle di posta destinate a ricevere i report aggregati giornalieri in formato XML dai server riceventi. Questa telemetria evidenzia gli indirizzi IP sorgente, l'esito dei controlli crittografici e gli attacchi di spoofing in corso.

### 5. Come può essere integrato questo strumento di verifica nelle pipeline di CI/CD?
Eseguendo guardian.py con l'opzione -j, l'utility restituisce un output JSON deterministico con punteggio di sicurezza e raccomandazioni. I flussi di integrazione continua possono analizzare tale output e bloccare il rilascio qualora il punteggio scenda sotto gli 85 punti.

***

## 5. Consulenza e Sicurezza di Posta in Italia

Per architettura di sicurezza informatica avanzata, modellazione delle minacce e gestione degli incidenti a Roma, Milano o Torino, contatta il team di **TAVARDT**.

***

## Link Utili

* **Portale Aziendale:** https://ag.tavardt.com/it/
* **Suite di Diagnostica:** https://tools.tavardt.com/it/
* **Contatto Tecnico:** https://ag.tavardt.com/it/contatto/

***

### Licenza
Rilasciato sotto licenza MIT.

***

© 2026 TAVARDT. Tutti i diritti riservati.
