
# Architecture

## System Architecture

[Describe the overall architecture of your system. Replace the Mermaid diagram below with your actual architecture.]

```mermaid
graph TD
    A[User / Browser] -->|HTTP| B[Frontend - React]
    B -->|REST API| C[Backend - FastAPI]
    C -->|SDK| D[watsonx.ai]
    C -->|Query| E[PostgreSQL]
    C -->|Publish| F[Slack Webhook]
    D -->|Inference Result| C
```

## Components

| Component | Technology | Responsibility |
|---|---|---|
| Frontend | [e.g., React 18] | [e.g., Dashboard UI, user interaction] |
| Backend API | [e.g., FastAPI] | [e.g., Business logic, orchestration] |
| AI / ML | [e.g., watsonx.ai] | [e.g., Anomaly scoring, classification] |
| Database | [e.g., PostgreSQL] | [e.g., Storing pipeline events and scores] |
| Notifications | [e.g., Slack API] | [e.g., Alerting on threshold breaches] |

## Data Flow

[Describe how data moves through your system from input to output.]

1. [e.g., Pipeline logs are ingested via a webhook from GitHub Actions]
2. [e.g., Logs are preprocessed and chunked into 512-token segments]
3. [e.g., Each chunk is sent to the watsonx.ai inference endpoint]
4. [e.g., Anomaly scores are stored in PostgreSQL]
5. [e.g., The React dashboard polls the API every 30 seconds to refresh]

## Security Considerations

[Note any security decisions relevant to the architecture — even if basic.]

- [e.g., API keys stored in environment variables, never committed to git]
- [e.g., All API routes require a Bearer token]
- [e.g., Database credentials rotated via IBM Secrets Manager]graph TD
    A[JSON Seed Files<br/>4 feeds, ~1700 alerts]
    B[Ingestion Layer<br/>ingest.py]
    C[Normalisation<br/>normalize.py<br/>CommonAlert schema]
    D[Correlation Engine<br/>correlate.py]

    E[Pass 1: Rule-Based<br/>shared indicators + time windows]
    F[Pass 2: DBSCAN<br/>sentence-transformers<br/>embeddings]

    G[Incident Clusters]

    H[Scoring + FP Reduction<br/>score.py<br/>risk score 0-100]

    I[MITRE ATT&CK Mapper<br/>mitre.py<br/>keyword matching]

    J[BLUF Generator<br/>bluf.py<br/>watsonx.ai Granite]

    K[SQLite DB<br/>threatlens.db]

    L[FastAPI REST API<br/>10 endpoints]

    M[React Dashboard<br/>shadcn/ui + React + Tailwind]

    A --> B
    B --> C
    C --> D

    D --> E
    D --> F

    E --> G
    F --> G

    G --> H
    H --> I
    I --> J
    J --> K
    K --> L
    L --> M

## Scalability Notes

[Optional: how would this scale beyond the hackathon prototype?]

[e.g., "The FastAPI backend is stateless and could be horizontally scaled behind a load balancer. The watsonx.ai calls are the bottleneck and would benefit from request batching."]
