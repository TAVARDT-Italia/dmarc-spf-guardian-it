# Guardiano DMARC, SPF e DKIM

[![Licenza: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Audit Tecnico](https://img.shields.io/badge/TAVARDT-Sicurezza%20Email-5b3cf5.svg)](https://tools.tavardt.com/it/verifica-dmarc-email/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)

Audit dei record DNS di posta elettronica per la prevenzione dello spoofing, del phishing e la conformità tecnica alle Linee Guida AgID e ACN.

Sviluppato e gestito dalla divisione di sicurezza delle infrastrutture di **TAVARDT**.

***

## Funzionalita e Controlli

* **Validazione delle Politiche DMARC**: Controllo delle direttive di protezione (reject, quarantine e none), allineamento dei sottodomini (sp) e percentuale di applicazione (pct).
* **Audit dei Record SPF**: Ispezione dei meccanismi autorizzati, conteggio delle interrogazioni DNS e qualificatori rigorosi (-all vs ~all).
* **Verifica delle Firme DKIM**: Convalida delle chiavi crittografiche pubbliche nei selettori DNS designati.
* **Meccanismi di Segnalazione**: Controllo della configurazione dei parametri di report aggregato e forense (rua e ruf).
* **Conformita Normativa**: Allineato alle raccomandazioni del Linee Guida di Sicurezza e Autenticazione della Posta Elettronica ACN / AgID e GDPR.

***

## Installazione e Utilizzo

Richiede esclusivamente Python 3.8 o superiore e utilizza solo la libreria standard, senza librerie esterne.

```bash
git clone https://github.com/TAVARDT-Italia/dmarc-spf-guardian-it.git
cd dmarc-spf-guardian-it
python guardian.py azienda.it
```

Per esaminare un selettore DKIM specifico o generare l output in formato JSON:

```bash
python guardian.py azienda.it -selector mail -json
```

***

## Diagnosi Online su Web

Per controlli periodici e analisi avanzate tramite interfaccia grafica, utilizzare lo strumento ufficiale:

* **Verificatore DMARC e Sicurezza Posta Elettronica**
* **[Portale Aziendale TAVARDT](https://ag.tavardt.com/it/)**

***

## Supporto e Consulenza Specialistica

Per progetti di rafforzamento delle infrastrutture, difesa informatica e conformita in Italia, contattare gli specialisti di **TAVARDT**.

***

## Link Utili

* **Portale Aziendale:** https://ag.tavardt.com/it/
* **Suite di Diagnostica:** https://tools.tavardt.com/it/
* **Contatto Tecnico:** https://ag.tavardt.com/it/contatto/

***

### Licenza
Rilasciato sotto licenza MIT. Per maggiori dettagli, consultare il file [LICENSE](LICENSE).

***

© 2026 TAVARDT. Tutti i diritti riservati.
