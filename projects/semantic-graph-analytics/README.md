# Semantic Graph Analytics for Email/Calendar Workload

## Summary
A semantic modeling approach for analyzing communication/workload (emails/calendar events) using clustering + meaning-based graph relations to produce KPI rollups and deviation signals. The platform transforms raw event and email data into structured knowledge graphs, enabling deep insights into organizational patterns, workload drivers, and operational efficiency.

## Problem
- Entity-only extraction misses "why" and "type of work".
- Need a structure that supports business questions (load drivers, deviations, bottlenecks).
- Fragmented data across emails and calendar events makes holistic analysis difficult.
- Manual correlation of communication patterns and workload is time-consuming and inconsistent.

## Solution Overview (Spec)
- Canonical event fields (source, time, title/description, status)
- Semantic labeling:
  - `reason_semantic` category (meaning)
  - cluster subtypes (within each reason)
- Rollups:
  - weekly KPIs per cluster (volume, duration, deviation)
- Graph layer:
  - nodes: reason/cluster/team/project/person/department
  - edges: contributes_to, blocks, depends_on, communicates_with (meaning-first)

## Architecture

### Multi-Stage Pipeline

**Events Pipeline (9 Stages):**
1. **Schema Setup:** Initialize PostgreSQL schema for event storage
2. **Event Export:** Export calendar events to Parquet format with enhanced payload extraction
3. **Semantic Classification:** Classify events into semantic categories (STANDUP, PLANNING, REVIEW_RETRO, ONE_ON_ONE, etc.)
4. **Clustering & Rollups:** Cluster events by semantic similarity and generate weekly rollups
5. **Evidence Preparation:** Calculate priority scores and evidence for analysis
6. **LLM Report Generation:** Generate bilingual (EN/ES) executive reports with actionable insights
7. **Summary Building:** Build comprehensive summaries with KPIs and metrics
8. **Database Storage:** Store all pipeline outputs in PostgreSQL for querying
9. **Department Analysis:** Link events to organizational structure and calculate department/people/position metrics

**Email Pipeline (11 Stages):**
1. **Message Export:** Export email/message data to Parquet format
2. **Enrichment:** Normalize senders, calculate response times, extract metadata
3. **Entity Extraction:** Fast regex-based entity extraction (emails, names, companies, projects)
4. **Neo4j Entity Graph:** Build lightweight graph for LLM context enrichment
5. **LLM Analysis:** Sentiment, intent, and topic analysis with graph context
6. **Clustering:** Thread, topic, and sender clustering
7. **Network Analysis:** Communication graph construction and centrality analysis
8. **Summary Building:** KPIs and executive summary generation
9. **Database Storage:** Bulk insert all analysis results
10. **Department Analysis:** Department/people/position-level metrics
11. **Full Neo4j Analysis:** Comprehensive graph analysis with community detection

### Graph Database Integration (Neo4j)

**Knowledge Graph Structure:**
- **Event Nodes:** Calendar events with metadata (title, description, duration, organizer, attendees)
- **Person Nodes:** Unique persons with email, name, role, department
- **Department Nodes:** Organizational structure and hierarchy
- **Cluster Nodes:** Semantic clusters of related events
- **Topic Nodes:** Extracted topics from LLM analysis
- **Entity Nodes:** Companies, projects, and other entities mentioned

**Relationship Types:**
- **ORGANIZES:** Person → CalendarEvent (who organized the event)
- **ATTENDS:** Person → CalendarEvent (who attended the event)
- **COMMUNICATES_WITH:** Person → Person (communication patterns with metrics)
- **BELONGS_TO:** Person → Department (organizational membership)
- **HANDS_OFF_TO:** Work handoff patterns between people
- **APPROVES_FOR:** Approval workflow relationships
- **CONTRIBUTES_TO:** Cluster → Cluster (semantic relationships)
- **DEPENDS_ON:** Event → Event (temporal dependencies)

**Graph Analysis Capabilities:**
- **Community Detection:** Identify communication clusters and organizational boundaries
- **Centrality Analysis:** Find key influencers and connectors in the organization
- **Pattern Discovery:** Discover recurring patterns in communication and workload
- **Relationship Mapping:** Map dependencies and correlations between events and people
- **Temporal Analysis:** Analyze patterns over time through graph traversal

### Semantic Classification

**Event Categories (reason_semantic):**
- **STANDUP:** Daily standup, quick status updates, blockers
- **PLANNING:** Sprint planning, roadmap planning, capacity planning
- **REVIEW_RETRO:** Sprint review, demo, retrospective, lessons learned
- **ONE_ON_ONE:** One-on-one meetings between manager and employee
- **INTERVIEW:** Candidate interviews, recruiting, technical screens
- **INCIDENT:** Incident response, outage bridge, postmortem
- **SALES_CUSTOMER:** Customer calls, sales demos, account reviews
- **TRAINING:** Training sessions, onboarding, workshops
- **ADMIN_SCHEDULING:** Scheduling, coordination, calendar logistics
- **STATUS_SYNC:** Weekly syncs, team status updates, project tracking
- **DECISION:** Decisions, sign-offs, approvals, go/no-go
- **OTHER:** Generic meetings with unclear purpose

**Clustering Approach:**
- **Semantic Similarity:** Use embeddings (nomic-embed-text) for semantic clustering
- **Hierarchical Clustering:** Multi-level clustering within semantic categories
- **Pattern Recognition:** Identify recurring meeting patterns and event families
- **Cluster Naming:** Unique identifiers following `<reason_id>:<version>:<cluster_number>` format

## Advanced Analytics

### KPI Rollups & Metrics

**Weekly KPIs per Cluster:**
- **Volume Metrics:** Total events, total minutes, events per week
- **Duration Metrics:** Average duration, duration variance, duration trends
- **Deviation Metrics:** Schedule deviations, duration variances, frequency deviations
- **Priority Scores:** Calculated priority based on volume, deviation, and impact
- **Trend Analysis:** Week-over-week comparisons and trend detection

**Department-Level Metrics:**
- **Absenteeism:** Cancellation rate, no-show rate, absence frequency
- **Deviation:** Schedule deviations, duration variances, time-slot changes
- **Efficiency:** Meeting hours/week, meeting density, focus time availability
- **Flow Friction:** Back-to-back meetings, cross-department complexity, fragmentation
- **Performance:** Project meeting count, decision velocity, collaboration breadth
- **Time Patterns:** Start time variances, duration overruns, schedule predictability

**Person-Level Analytics:**
- **Meeting Patterns:** Individual meeting frequency, duration, and types
- **Collaboration Metrics:** Collaboration breadth, cross-department engagement
- **Cancellation Patterns:** Cancellation rates and no-show patterns
- **Time Utilization:** Meeting hours, focus time, schedule efficiency

### Pattern Detection

**Recurring Patterns:**
- **Recurring Meetings:** Identify meetings with same title patterns (3+ occurrences)
- **Project Patterns:** Detect project-related events through keyword matching
- **Location Patterns:** Common meeting rooms and resource utilization
- **Temporal Patterns:** Time-based patterns (daily standups, weekly syncs)

**Anomaly Detection:**
- **Volume Anomalies:** Unusual spikes or drops in meeting volume
- **Duration Anomalies:** Meetings significantly longer or shorter than normal
- **Pattern Deviations:** Deviations from established meeting patterns
- **Cross-Department Anomalies:** Unusual collaboration patterns

**Network Analysis:**
- **Key Influencers:** Top people by outgoing communication volume
- **Key Connectors:** Top people by connection count (betweenness centrality)
- **Department Collaboration:** Cross-department communication patterns
- **Handoff Chains:** Workflow patterns and task handoffs
- **Approval Networks:** Approval request and response patterns

## LLM-Powered Analysis

### AI-Generated Reports

**Bilingual Executive Summaries:**
- **Language Support:** Generate reports in English and Spanish
- **Executive Insights:** High-level summaries with actionable recommendations
- **Trend Analysis:** Week-over-week comparisons and trend identification
- **Anomaly Highlighting:** Automatic identification of significant deviations

**LLM Integration (Ollama):**
- **Model:** deepseek-r1:1.5b (CPU-friendly) or deepseek-r1:8b (higher quality)
- **Embeddings:** nomic-embed-text for semantic similarity
- **Context Enrichment:** Graph context enhances LLM prompts for better accuracy
- **Batch Processing:** Optimized batch processing with parallel workers and caching

**Report Features:**
- **Semantic Understanding:** Deep understanding of event context and relationships
- **Pattern Recognition:** Identify patterns that would be difficult to detect manually
- **Recommendations:** Actionable recommendations based on analysis
- **Multi-Dimensional Analysis:** Combine multiple data sources for comprehensive insights

## Data Storage & Querying

### Parquet Format

**Storage Strategy:**
- **Partitioning:** Partition by tenant, date, and event type for efficient querying
- **Schema Versioning:** Support schema evolution while maintaining backward compatibility
- **Compression:** High compression ratios (5-10x) for efficient storage
- **Columnar Format:** Optimized for analytical queries and aggregations

### PostgreSQL Integration

**Database Tables:**
- **Stage Tables:** Separate tables for each pipeline stage (stage_0 through stage_7)
- **Summary Tables:** Aggregated summaries with KPIs and metrics
- **Department Tables:** Department, people, and position-level analytics
- **Pattern Tables:** Identified patterns and recurring events

**Query Capabilities:**
- **Time-Series Queries:** Efficient time-range queries with partition pruning
- **Aggregation Queries:** Fast aggregations across multiple dimensions
- **Join Queries:** Efficient joins between events, people, and departments
- **Filtering:** Rich filtering capabilities by tenant, department, person, time range

### API Endpoints

**Analysis Endpoints:**
- **Summary:** Executive summaries with KPIs
- **Time-Series:** Weekly time-series data for visualization
- **Metrics:** Department, people, and position-level metrics
- **Patterns:** Identified patterns and recurring events
- **Network:** Communication network analysis

**Query Features:**
- **Multi-Tenant Support:** Data isolation per tenant/organization
- **Flexible Filtering:** Filter by date range, department, person, cluster
- **Predefined Periods:** Shortcuts for common time periods (1week, 3months, etc.)
- **Bilingual Support:** Reports available in English and Spanish

## Performance Optimizations

### Pipeline Optimizations

**Incremental Processing:**
- **Incremental Mode:** Skip already processed events using tracking tables
- **Rebuild Mode:** Force reprocessing when needed for schema changes
- **Deduplication:** Strong database uniqueness constraints prevent duplicates
- **Historical Scope Control:** Limit processing to recent weeks for efficiency

**Neo4j Optimizations:**
- **Entity Pre-Extraction:** Extract entities once in early stages, reuse in later stages
- **Connection Pooling:** Reuse Neo4j connections (pool size: 50, timeout: 120s)
- **Batch Processing:** Increased batch sizes (1000) for better performance
- **Index Optimization:** Strategic indexes on email, tenant_id, start_at

**LLM Optimizations:**
- **Batch Processing:** Process messages in batches (default: 20 per batch)
- **Parallel Workers:** Parallel processing with configurable worker count
- **Caching:** In-memory caching for frequently accessed data
- **JSON Mode:** Pre-compiled regex patterns for faster processing

### Query Performance

**Database Optimizations:**
- **Partition Pruning:** Automatic partition pruning for time-range queries
- **Index Usage:** Strategic indexes for common query patterns
- **Query Optimization:** PostgreSQL query planner optimizations
- **Connection Pooling:** Efficient database connection management

**Graph Query Optimization:**
- **Graph Traversal:** Efficient graph traversal algorithms
- **Relationship Caching:** Cache frequently accessed relationships
- **Parallel Execution:** Parallel graph queries where possible
- **Result Limiting:** Limit result sets for large graphs

## Deliverables
- Data schema spec (events + semantic labels + rollups)
- KPI definitions and aggregation rules
- Query examples (what drives meeting load, where deviations increase)
- **Graph database schema** (Neo4j nodes and relationships)
- **Pipeline architecture** (multi-stage processing pipeline)
- **API specification** (REST endpoints for analysis)
- **Department/people analytics** (organizational-level insights)
- **Pattern detection algorithms** (recurring patterns and anomalies)

## Tech
Python, Parquet, PostgreSQL, Neo4j, Apache Airflow, Ollama (LLM), FastAPI, clustering algorithms, graph algorithms, embeddings (nomic-embed-text)

## Business Value

**Operational Insights:**
- **Workload Analysis:** Understand what drives meeting load and identify bottlenecks
- **Efficiency Optimization:** Identify opportunities to reduce meeting overhead
- **Pattern Recognition:** Discover recurring patterns and optimize scheduling
- **Department Performance:** Compare department metrics and identify best practices

**Strategic Value:**
- **Resource Planning:** Data-driven resource allocation based on workload patterns
- **Process Improvement:** Identify process inefficiencies and improvement opportunities
- **Collaboration Analysis:** Understand collaboration patterns and optimize team structures
- **Predictive Analytics:** Predict workload trends and plan accordingly

**Decision Support:**
- **Executive Reporting:** Automated executive summaries with actionable insights
- **Anomaly Detection:** Early detection of unusual patterns requiring attention
- **Benchmarking:** Compare metrics across departments, positions, and time periods
- **Root Cause Analysis:** Understand drivers of workload and deviations

## Algorithms & Methodologies

### Embedding & Semantic Similarity

**Cosine Similarity:**
- **Purpose:** Classifies calendar events by comparing event text embeddings against prototype embeddings (12 semantic categories)
- **Implementation:** Uses `nomic-embed-text` model (768 dimensions) via Ollama API
- **Process:** Batch processing (default: 64 events per batch), similarity threshold: 0.38
- **Prototype-Based Classification:** Pre-defined bilingual prototypes for each semantic category, events classified by highest cosine similarity

**Vector Similarity Search (Neo4j):**
- **Algorithm:** Vector index-based K-nearest neighbors (KNN) using cosine similarity
- **Implementation:** Creates vector index (768 dimensions), uses `db.index.vector.queryNodes()` for fast similarity search
- **Configuration:** Top-K: 10 neighbors, threshold filtering for relationship creation

### Clustering Algorithms

**Leiden Algorithm (Graph Data Science):**
- **Purpose:** Groups documents into clusters based on similarity relationships
- **Process:** Creates undirected graph projection, runs Leiden clustering, assigns `communityId` to documents
- **Application:** Knowledge island detection and expert identification
- **Reference:** Traag, V. A., Waltman, L., & van Eck, N. J. (2019). From Louvain to Leiden: guaranteeing well-connected communities.

**Thread-Based Clustering:**
- **Algorithm:** Groups messages by `thread_id`
- **Metrics:** Thread duration, participant count, dominant sentiment, thread size
- **Output:** Identifies conversation threads and calculates thread-level metrics

**Topic-Based Clustering:**
- **Algorithm:** Groups messages sharing common topics from LLM extraction
- **Process:** Extracts topics from LLM-generated JSON, builds topic → messages mapping, creates clusters for topics with minimum size
- **Features:** Handles overlapping topics, extracts top terms, calculates participant diversity

**Sender-Based Clustering:**
- **Algorithm:** Groups messages by normalized sender email
- **Purpose:** Identifies communication patterns by individual senders
- **Metrics:** Unique recipients, sentiment/intent distribution, average urgency score

### Graph Algorithms

**Community Detection (Leiden):**
- **Purpose:** Identifies document clusters based on similarity graph
- **Process:** Input graph with `SIMILAR_TO` relationships, outputs `DocumentCluster` nodes with `communityId`
- **Relationships:** Creates `IN_CLUSTER` relationships from documents to clusters

**Pattern Detection (Cypher Reasoning):**
- **Potential Collaborators:** Documents in same cluster with high similarity, different owners
- **Content Duplication:** Documents with similarity > 0.95
- **Knowledge Islands:** Clusters where >80% documents owned by one person
- **Expert Identification:** Person who owns most documents in a cluster

### Network Analysis

**Communication Graph Construction:**
- **Algorithm:** Directed graph from sender → receiver relationships
- **Edge Properties:** Message count, average response time, first/last contact, relationship type, dominant sentiment
- **Relationship Types:** Frequent (>10 messages), Occasional (3-10), Rare (<3)

**Centrality Analysis:**
- **Algorithms:** Degree centrality (in-degree, out-degree, total degree)
- **Metrics:** In-degree (incoming messages), out-degree (outgoing messages), total degree, normalized centrality score
- **Purpose:** Identifies key connectors and communication hubs

**Response Time Analysis:**
- **Algorithm:** Statistical aggregation of response times
- **Metrics:** Average response time per sender-receiver pair, response time distribution, SLA tracking
- **Application:** Identifies slow responders and communication bottlenecks

**Handoff Pattern Detection:**
- **Algorithm:** Thread-based forwarding chain analysis
- **Purpose:** Identifies message handoffs and escalation patterns
- **Process:** Analyzes thread sequences, detects forwarding/reply patterns, tracks escalation paths

### Natural Language Processing

**LLM-Based Analysis (DeepSeek R1):**
- **Model:** `deepseek-r1:1.5b` (CPU-friendly) or `deepseek-r1:8b` (higher quality)
- **Email Analysis:** Sentiment analysis, intent classification, topic extraction, entity extraction, urgency scoring, business objective detection
- **Event Analysis:** Executive summary generation, bilingual output, KPI analysis, trend analysis
- **Document Analysis:** Cluster summaries, global summaries, bilingual output, pattern highlighting
- **Optimizations:** Batch processing (20 messages per batch), parallel workers (4 threads), LRU cache with TTL, early exit for simple messages

**Embedding Generation:**
- **Model:** `nomic-embed-text` (768 dimensions)
- **Purpose:** Converts text to dense vector representations
- **Process:** Batch processing via Ollama API, text truncation (4000 chars max), returns 768-dimensional vectors

### Entity Extraction

**Regex-Based Entity Extraction (Fast):**
- **Algorithm:** Pre-compiled regex patterns for structured extraction
- **Entities:** Email addresses, person names, @mentions, company names, project references, ticket/issue IDs, monetary amounts, dates, URLs, phone numbers
- **Performance:** 10-100x faster than LLM-based extraction
- **Purpose:** Fast structural extraction for Neo4j graph enrichment before LLM analysis

**LLM-Based Entity Extraction:**
- **Algorithm:** DeepSeek R1 model extracts entities from natural language
- **Entities:** People, companies, projects, locations, dates/times, custom domain-specific entities
- **Output:** JSON structure with entity types and values

### Statistical Analysis

**Time Series Aggregation:**
- **Algorithms:** Weekly rollups and time-series calculations
- **Metrics:** Week-over-week (WoW) change, month-over-month (MoM) change, moving averages, cumulative totals
- **Aggregations:** Sum, average, min, max per week/month, grouped by tenant, cluster, reason, department

**Priority Scoring:**
- **Algorithm:** Multi-factor priority scoring
- **Factors:** Total meeting minutes, number of events, trend direction, volatility, week-over-week change
- **Purpose:** Ranks event clusters by business impact

**Department Metrics:**
- **Absenteeism:** Cancellation rate, no-show rate, absence frequency, attendance patterns
- **Deviation:** Schedule deviations, duration variances, frequency deviations, time-slot changes
- **Efficiency:** Meeting hours per week, meeting density, focus time availability, time utilization
- **Flow Friction:** Back-to-back meetings, cross-department complexity, fragmentation index, context-switching frequency
- **Performance:** Project meeting count, decision velocity, response time, collaboration breadth

### Time Series Analysis

**Weekly Rollups:**
- **Algorithm:** Temporal aggregation by ISO week
- **Process:** Extract year/week from timestamps, group by `(cluster_uid, week_key)`, calculate aggregations
- **Output:** `ClusterRollup` nodes in Neo4j, linked to `Week` nodes

**Trend Analysis:**
- **Algorithms:** Linear regression and variance calculation
- **Metrics:** Trend (slope of linear fit), volatility (standard deviation), last week metrics, weeks analyzed
- **Purpose:** Identifies increasing/decreasing patterns and stability

**Lookback Windows:**
- **Algorithm:** Time-based filtering using ISO week calculations
- **Configuration:** `LOOKBACK_WEEKS=N` environment variable
- **Purpose:** Limits analysis to recent data (e.g., last 12 weeks)

### Deduplication & Incremental Processing

**Tracking-Based Deduplication:**
- **Algorithm:** Database-backed tracking table for processed events
- **Process:** Checks `pipeline_processed_events` table, filters already-processed events, marks processed events after insertion
- **Purpose:** Skips re-processing in incremental runs

**Database-Level Upserts:**
- **Algorithm:** PostgreSQL `ON CONFLICT DO UPDATE` (upsert)
- **Unique Constraints:** Stage-specific constraints prevent duplicate rows across pipeline reruns
- **Application:** Events pipeline (stage 0-2), Email pipeline (stage 0-5), Documents pipeline (stage 2-3)

### Performance Optimizations

**Batch Processing:**
- Embeddings: 64 events per batch (configurable)
- LLM Analysis: 20 messages per batch (configurable)
- Neo4j Upserts: 1000 nodes per batch

**Connection Pooling:**
- Neo4j: Pool size 50, timeout 120s
- HTTP: Connection reuse for Ollama API calls
- PostgreSQL: Connection pooling via psycopg

**Caching:**
- LLM Cache: LRU cache with 1-hour TTL
- Hash-based: Content hash for cache keys
- Hit Rate: 40-50% for duplicate content

**Parallel Processing:**
- LLM Workers: 4 concurrent threads (configurable)
- I/O Parallelism: Non-blocking API calls
- Thread Pool: `ThreadPoolExecutor` for batch processing

**Early Exit Strategies:**
- Skip LLM for simple messages ("Thanks!", "OK")
- Skip processing if all events already tracked
- Skip empty dataframes

### Algorithm Complexity

| Algorithm | Time Complexity | Space Complexity | Notes |
|-----------|----------------|------------------|-------|
| Cosine Similarity | O(n) | O(1) | n = vector dimension (768) |
| Leiden Clustering | O(n log n) | O(n) | n = number of nodes |
| Vector KNN Search | O(log n) | O(n) | With vector index |
| Thread Clustering | O(n) | O(n) | n = messages |
| Topic Clustering | O(n×m) | O(n×m) | n = messages, m = topics per message |
| Network Graph Build | O(n×k) | O(n×k) | n = messages, k = avg recipients |
| Centrality Calculation | O(n+m) | O(n+m) | n = nodes, m = edges |
| LLM Batch Processing | O(b×t) | O(b) | b = batch size, t = tokens |

## Next Steps
- Train similarity models on synthetic datasets to validate methodology
- Integrate summarization model to generate weekly narrative reports
- Expand graph analysis to include document relationships and knowledge islands
- Implement real-time pattern detection for proactive workload management
- Develop predictive models for workload forecasting and capacity planning
- Enhance department analysis with position-level benchmarking
- Integrate with external calendar systems for automated data ingestion
- **Algorithm Enhancements:** Hierarchical clustering for multi-level organization, topic modeling (LDA/BERTopic), anomaly detection (Isolation Forest/LSTM), predictive analytics for meeting trends, graph neural networks for advanced relationship learning
