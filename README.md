## Victor Ocampo Marin

**Data Engineer** based in Costa Rica. I build data-intensive systems with **SQL Server**, **ETL (SSIS, Apache NiFi)** and **Python**, and I'm currently expanding into **Databricks** and **Apache Spark**.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/victor-ocampo-marin-69466122b)
[![Email](https://img.shields.io/badge/Email-EA4335?style=flat&logo=gmail&logoColor=white)](mailto:victorocampomarin21@gmail.com)

---

### About me

- 5 years in software and data: ETL pipelines, data ingestion, migrations, SQL tuning and cloud deployments on Azure.
- Today I'm responsible for data development across **three concurrent client projects** at a data outsourcing firm: data ingestion, file migration and SSIS migration.
- I design and build the **Apache NiFi** flows for two of those projects, including a pipeline that loads virtual machine logs into a database for analytics that feeds an AI agent.
- Before data engineering, I was a full stack engineer (C#/.NET, Go, Vue.js, Django), so I care about the whole path: from the source system to the person reading the dashboard.
- Spanish (native) · English (B2+, professional working proficiency).

### What I'm working on

- **Lakehouse project (in progress):** a Databricks lakehouse that ingests synthetic Costa Rican electronic invoices (XML), processes them through a medallion architecture (Bronze → Silver → Gold) with PySpark, applies data quality rules and models a star schema. → [`datalakehouse-practice`](https://github.com/VictorOcampo21/datalakehouse-practice)
- **Learning:** Databricks, Apache Spark (PySpark), Delta Lake and Microsoft Fabric.

### Tech stack

**Data engineering**
![SQL Server](https://img.shields.io/badge/SQL_Server-CC2927?style=flat&logo=microsoftsqlserver&logoColor=white)
![SSIS](https://img.shields.io/badge/SSIS-5C2D91?style=flat&logo=microsoft&logoColor=white)
![Apache NiFi](https://img.shields.io/badge/Apache_NiFi-728E9B?style=flat&logo=apache&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?style=flat&logo=snowflake&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)

**Languages**
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL_(T--SQL)-336791?style=flat)
![C#](https://img.shields.io/badge/C%23-512BD4?style=flat&logo=dotnet&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)

**Cloud & tools**
![Azure](https://img.shields.io/badge/Azure-0078D4?style=flat&logo=microsoftazure&logoColor=white)
![Bash](https://img.shields.io/badge/Bash_scripting-4EAA25?style=flat&logo=gnubash&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)

**Currently learning**
![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=flat&logo=databricks&logoColor=white)
![Apache Spark](https://img.shields.io/badge/Apache_Spark-E25A1C?style=flat&logo=apachespark&logoColor=white)
![Microsoft Fabric](https://img.shields.io/badge/Microsoft_Fabric-117865?style=flat&logo=microsoft&logoColor=white)

---

### Selected work

Most of my production work belongs to clients or organizations and its source code is private. Here is what I built, described without confidential details.

<details>
<summary><b>FactuBot</b>: ETL bot for electronic invoices (Python, Django, Azure Database for PostgreSQL)</summary>

<br>

ETL bot used by multiple accountants, installed on each user's computer and backed by Azure Database for PostgreSQL. It collects electronic invoices from email, validates them against the requirements of Costa Rica's tax authority (Hacienda) and turns them into tax and sales KPIs.

- **Scheduled ingestion:** checks Gmail through the Gmail API daily by default, with a user-configurable schedule.
- **No duplicates:** queue-based processing keyed on each invoice's unique identifier; once processed, an XML is never picked up again.
- **Validation:** every mandatory field of Hacienda's XML structure, plus the presence of Hacienda's acceptance message for each invoice. Invoices that fail go to an error queue for review.
- **Data model:** Azure Database for PostgreSQL with separate header and line-item tables, so multi-line invoices are stored at line level.
- **KPIs:** VAT and other taxes, profit and loss, and top-selling items by category when the invoice includes one.

```mermaid
flowchart LR
    A[Gmail API<br/>scheduled check] --> B[Download XML]
    B --> C{Already<br/>processed?}
    C -- yes --> S[Skip]
    C -- no --> D{Mandatory fields and<br/>Hacienda acceptance<br/>message present?}
    D -- no --> Q[Error queue<br/>for review]
    D -- yes --> E[(PostgreSQL<br/>header + line items)]
    E --> F[KPI dashboard<br/>VAT, taxes, P&L,<br/>top sellers]
```

</details>

<details>
<summary><b>OftaData</b>: multi-clinic patient records system in production (Go, Vue.js, PostgreSQL, Docker, Azure)</summary>

<br>

Built end to end for an ophthalmologist who works across five clinics in Costa Rica: database, backend, frontend and deployment. It centralizes every patient from every clinic in one system (1,000+ patient forms with medical images), used daily by the doctor and the reception staff.

- **Clinical workflow:** reception registers the patient or their arrival and opens the visit form; the doctor completes it during the consultation and attaches images or scanned paper documents.
- **Data model:** each patient has a profile, a clinical history (consultation forms) and a surgical history (signed surgery forms).
- **Images:** uploaded from the browser, compressed and optimized, stored on the VM's disk, with their paths kept in PostgreSQL.
- **Backups:** daily automated backups of the database and the images with 7-day retention, already used to restore data after real incidents.
- **Security:** role-based access, HTTPS with an SSL certificate, firewall rules on Azure, and controlled SSH access.
- **Appointments:** appointment calendar with WhatsApp reminders to patients.

```mermaid
flowchart LR
    A[Reception<br/>registers visit] --> B[Doctor completes<br/>consultation form]
    B --> C[Upload images or<br/>scanned documents]
    C --> D[Go backend]
    D --> E[Compress and<br/>optimize images]
    E --> F[VM disk<br/>image files]
    D --> G[(PostgreSQL<br/>patients, forms, image paths)]
    subgraph Azure VM
        D
        E
        F
        G
        H[Daily backups<br/>database + images]
    end
    F -.-> H
    G -.-> H
```

</details>

<details>
<summary><b>Metadata-driven file migration with integrity checks</b> (Apache NiFi, Python, SQL Server, Isilon)</summary>

<br>

Client project for a government judicial entity; details generalized for confidentiality. A NiFi flow migrates documents from source file repositories to Isilon storage, driven by a SQL Server metadata table that holds each file's specifications and location.

- **Validation:** Python scripts detect corrupted or encrypted files (PDF and other formats), which are not allowed in the flow.
- **Correct naming:** files are renamed by their file ID, and a Python script verifies the renaming.
- **Integrity check:** SHA-256 hash computed in NiFi at the source and verified at the destination.
- **Resilience:** failed transfers go to a retry queue capped at a maximum number of attempts.
- **Error handling:** every file that fails a check (corrupted, encrypted, or out of retries) is logged to an error table with an error status, so the client can review it and decide whether to re-fetch it from the source or discard it.
- **Traceability:** hash, size and validation results are written to SQL Server audit tables.

```mermaid
flowchart LR
    A[(SQL Server<br/>file metadata)] --> B[Read file specs<br/>as JSON attributes]
    B --> C[Fetch file from<br/>source repository]
    C --> D{Python check:<br/>corrupted or<br/>encrypted?}
    D -- yes --> Z
    D -- no --> E[Compute SHA-256<br/>in NiFi]
    E --> F[Copy to Isilon<br/>renamed by file ID]
    F --> G{Hash match and<br/>name verified?}
    G -- no --> R[Retry queue<br/>max N attempts]
    R -- retry --> F
    R -- limit reached --> Z[(Error log<br/>status: error, for review)]
    G -- yes --> H[(Audit log<br/>hash, size, validations)]
```

</details>

<details>
<summary><b>Other client data projects</b>: ingestion and migration (Apache NiFi, SSIS)</summary>

<br>

- Log ingestion flow (in progress) for a government judicial entity: Apache NiFi collects application logs from multiple virtual machines, extracts the key fields and loads them into a database that the analytics team uses to feed KPIs to an AI support agent.
- Modernization of legacy SSIS ETL and SQL processes for a regional financial institution: adapting existing packages to new tables and servers, troubleshooting incremental loads and resolving data tickets.

</details>

---

### Certifications

- [Microsoft Certified: Azure Fundamentals (AZ-900)](https://learn.microsoft.com/es-mx/users/victormarin-8450/credentials/7c36f512f6349db9)
- [Oracle APEX Cloud Developer Certified Professional](https://catalog-education.oracle.com/ords/certview/sharebadge?id=48671B17B024B2A35CF7ED80C73E3FB2B38FF3BC221E991F5590BA8FC01C1BF1)
- [Certified Scrum Master, International Scrum Institute](https://www.scrum-institute.org/certifications/Scrum-Institute.Org-SMACea023cafa7-87462744894437.pdf)

### Let's connect

Open to Data Engineer opportunities (remote, hybrid or on-site). Reach me on [LinkedIn](https://www.linkedin.com/in/victor-ocampo-marin-69466122b) or at [victorocampomarin21@gmail.com](mailto:victorocampomarin21@gmail.com).
