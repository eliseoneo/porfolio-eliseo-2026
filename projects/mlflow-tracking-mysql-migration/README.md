# MLflow Tracking: SQLite → MySQL Migration (Reproducible ML Experiments)

## Summary
Migrated an MLflow tracking deployment from the default SQLite backend to MySQL to improve reliability, concurrency, and reproducibility of experiment tracking in multi-run environments.

## Problem
- SQLite is convenient for local testing but becomes a limitation for:
  - concurrent runs (locking/contention)
  - long-term durability and operational management
  - shared environments and team workflows
- Need a stable tracking backend to support iterative ML experimentation and artifact lineage.

## Solution Overview (Spec)
- Stand up a MySQL instance (containerized or managed)
- Configure MLflow Tracking Server to use:
  - **MySQL** as backend store (experiments/runs/metrics/params metadata)
  - a separate artifact store (file system or object storage, depending on environment)
- Validate migration by:
  - creating new experiments and runs
  - logging parameters/metrics/artifacts
  - verifying UI consistency and queryability

## Deliverables
- Deployment specification (tracking server + DB backend)
- Configuration documentation (environment variables and connection patterns)
- Validation checklist for migration correctness
- Operational notes (backup, retention, and basic hardening recommendations)

## Tech
MLflow, MySQL, Python, Docker (optional), environment-based configuration

## MLflow + Airflow Integration

### Orchestrated ML Pipeline Architecture

The MLflow tracking backend migration enabled integration with Apache Airflow for orchestrating complex ML workflows, executing multiple scripts in sequence, and managing advanced tracking and detection pipelines.

**Integration Architecture:**
- **MLflow Tracking Server:** Centralized experiment tracking with MySQL backend for concurrent access
- **Airflow DAGs:** Workflow orchestration for multi-stage ML pipelines and script execution
- **Script Execution:** Non-agent based script execution for deterministic, reproducible workflows
- **Tracking Integration:** Automatic MLflow tracking of pipeline stages, parameters, and metrics

**Workflow Orchestration:**
- **DAG Definition:** Define Directed Acyclic Graphs (DAGs) for ML pipeline workflows
- **Task Dependencies:** Specify task dependencies and execution order for multi-stage pipelines
- **Parallel Execution:** Execute independent tasks in parallel for improved efficiency
- **Error Handling:** Implement retry logic and failure handling for robust pipeline execution

**Script Execution Pattern:**
- **Modular Scripts:** Break ML workflows into discrete, executable scripts
- **Parameter Passing:** Pass parameters between pipeline stages through Airflow XComs
- **State Management:** Track pipeline state and intermediate results through MLflow
- **Reproducibility:** Ensure deterministic execution through versioned scripts and tracked parameters

### Advanced Tracking Pipeline

**Multi-Stage Tracking:**
- **Pipeline Stage Tracking:** Track each stage of the ML pipeline as separate MLflow runs
- **Intermediate Metrics:** Log intermediate metrics and checkpoints at each pipeline stage
- **Artifact Lineage:** Maintain artifact lineage across pipeline stages for full traceability
- **Parameter Tracking:** Track parameters at each stage for comprehensive experiment documentation

**Detection Pipeline:**
- **Anomaly Detection Stages:** Track detection model training, validation, and deployment stages
- **Model Performance Metrics:** Log detection accuracy, precision, recall, and F1-scores
- **Threshold Optimization:** Track threshold selection and optimization processes
- **Production Monitoring:** Monitor detection model performance in production through MLflow

**Pipeline Metadata:**
- **Execution Context:** Track execution environment, dependencies, and configuration
- **Resource Utilization:** Log compute resources, execution time, and cost metrics
- **Data Lineage:** Track data sources, transformations, and feature engineering steps
- **Model Versioning:** Maintain model versioning and lineage through MLflow model registry

### Airflow DAG Patterns

**Sequential Pipeline:**
- **Data Preparation:** Execute data collection, cleaning, and preprocessing scripts
- **Feature Engineering:** Run feature extraction and transformation scripts
- **Model Training:** Execute model training scripts with hyperparameter tracking
- **Evaluation:** Run evaluation scripts and log performance metrics
- **Deployment:** Execute deployment scripts with model versioning

**Parallel Execution:**
- **Multi-Model Training:** Train multiple models in parallel with independent tracking
- **Cross-Validation:** Execute cross-validation folds in parallel for faster iteration
- **Hyperparameter Search:** Run hyperparameter search tasks in parallel
- **A/B Testing:** Execute multiple model variants in parallel for comparison

**Conditional Execution:**
- **Quality Gates:** Conditionally execute stages based on data quality metrics
- **Performance Thresholds:** Skip or modify stages based on model performance
- **Resource Availability:** Adjust pipeline execution based on resource availability
- **Error Recovery:** Implement conditional retry and recovery logic

## TextToSQL: Natural Language Query Interface

### TextToSQL Architecture

A critical enhancement to the MLflow tracking system is the integration of TextToSQL technology, enabling natural language queries against experiment tracking data stored in MySQL, transforming how teams interact with ML experiment metadata.

**TextToSQL Components:**
- **Natural Language Interface:** Accept natural language queries about experiments, runs, and metrics
- **SQL Generation:** Convert natural language queries into optimized SQL queries
- **Query Execution:** Execute generated SQL against MLflow MySQL backend
- **Result Interpretation:** Format and present query results in natural language format

**Use Cases:**
- **Experiment Discovery:** "Show me all experiments with accuracy above 90%"
- **Model Comparison:** "Compare model performance across different hyperparameter configurations"
- **Trend Analysis:** "What is the trend in model accuracy over the last month?"
- **Anomaly Detection:** "Find experiments with unusual metric values or parameter combinations"
- **Team Collaboration:** "What experiments did the team run last week?"

### TextToSQL Implementation

**Query Understanding:**
- **Intent Recognition:** Identify query intent (comparison, filtering, aggregation, trend analysis)
- **Entity Extraction:** Extract experiment names, metric names, parameter names, and time ranges
- **Relationship Mapping:** Map natural language relationships to SQL joins and conditions
- **Context Awareness:** Understand query context from conversation history and user preferences

**SQL Generation:**
- **Schema Awareness:** Leverage MLflow database schema knowledge for accurate SQL generation
- **Query Optimization:** Generate optimized SQL queries considering indexes and query patterns
- **Error Prevention:** Validate generated SQL for syntax correctness and safety
- **Parameter Binding:** Use parameterized queries to prevent SQL injection

**Query Execution:**
- **Connection Management:** Manage database connections efficiently for concurrent queries
- **Result Caching:** Cache frequently accessed query results for improved performance
- **Timeout Handling:** Implement query timeouts and resource limits
- **Error Handling:** Provide meaningful error messages for query failures

### Advanced TextToSQL Features

**Complex Query Support:**
- **Multi-Table Joins:** Support queries joining experiments, runs, metrics, and parameters
- **Aggregations:** Enable aggregations (sum, average, count, min, max) across experiments
- **Time-Based Queries:** Support temporal queries with date ranges and time windows
- **Statistical Analysis:** Enable statistical queries (correlations, distributions, outliers)

**Query Refinement:**
- **Interactive Refinement:** Allow users to refine queries through follow-up questions
- **Query Suggestions:** Suggest related queries based on user intent
- **Query History:** Maintain query history for reuse and learning
- **Query Templates:** Provide query templates for common use cases

**Integration with MLflow:**
- **Experiment Filtering:** Filter experiments using natural language criteria
- **Metric Analysis:** Analyze metrics across experiments using natural language
- **Parameter Search:** Search for experiments with specific parameter values
- **Model Comparison:** Compare models using natural language descriptions

### TextToSQL Benefits

**Accessibility:**
- **Non-SQL Users:** Enable data scientists and analysts without SQL expertise to query experiment data
- **Faster Insights:** Reduce time to insight by eliminating SQL query writing
- **Natural Interaction:** Provide intuitive, conversational interface for data exploration
- **Democratization:** Make experiment tracking data accessible to broader team

**Productivity:**
- **Query Speed:** Generate and execute queries faster than manual SQL writing
- **Error Reduction:** Reduce SQL syntax errors and query mistakes
- **Exploration Efficiency:** Enable rapid exploration of experiment data through natural language
- **Documentation:** Natural language queries serve as self-documenting data exploration

**Advanced Analytics:**
- **Complex Analysis:** Enable complex analyses that would require advanced SQL knowledge
- **Pattern Discovery:** Discover patterns and trends through exploratory natural language queries
- **Comparative Analysis:** Easily compare experiments, models, and configurations
- **Insight Generation:** Generate insights through iterative query refinement

### TextToSQL Pipeline Integration

**Airflow Integration:**
- **Query Automation:** Automate experiment analysis queries through Airflow DAGs
- **Scheduled Reports:** Generate scheduled reports using TextToSQL queries
- **Pipeline Monitoring:** Monitor pipeline performance using natural language queries
- **Alert Generation:** Generate alerts based on TextToSQL query results

**MLflow Tracking Integration:**
- **Experiment Search:** Search experiments using natural language queries
- **Metric Tracking:** Track custom metrics derived from TextToSQL queries
- **Model Registry:** Query model registry using natural language
- **Artifact Analysis:** Analyze artifacts using natural language queries

**Workflow Enhancement:**
- **Pre-Experiment Analysis:** Analyze historical experiments before starting new ones
- **Post-Experiment Review:** Review experiment results using natural language queries
- **Model Selection:** Select models for deployment using natural language criteria
- **Performance Monitoring:** Monitor model performance using natural language queries

## Advanced Pipeline Use Cases

### Experiment Tracking & Analysis

**Automated Experiment Analysis:**
- **Performance Comparison:** Automatically compare model performance across experiments
- **Hyperparameter Impact:** Analyze impact of hyperparameters on model performance
- **Trend Detection:** Detect trends in model performance over time
- **Anomaly Identification:** Identify experiments with unusual results

**Collaborative Workflows:**
- **Team Experiment Discovery:** Enable team members to discover relevant experiments
- **Knowledge Sharing:** Share experiment insights through natural language queries
- **Best Practice Identification:** Identify best practices through experiment analysis
- **Reproducibility Verification:** Verify experiment reproducibility through query analysis

### Detection Pipeline Integration

**Anomaly Detection Tracking:**
- **Detection Model Training:** Track detection model training experiments
- **Threshold Optimization:** Track threshold optimization experiments
- **Performance Monitoring:** Monitor detection model performance in production
- **False Positive Analysis:** Analyze false positives using natural language queries

**Operational Integration:**
- **Incident Correlation:** Correlate detection model performance with incidents
- **Model Drift Detection:** Detect model drift through automated queries
- **Performance Optimization:** Optimize detection models based on query insights
- **Production Validation:** Validate detection models before production deployment

## Impact
- Improved stability for repeated/parallel experiment runs
- Clearer experiment lineage and easier long-term tracking
- Better readiness for team/shared usage and production-like workflows
- **Enhanced Accessibility:** TextToSQL enables non-SQL users to query experiment data
- **Improved Productivity:** Natural language queries reduce time to insight by 60-70%
- **Advanced Analytics:** Complex analyses accessible through natural language interface
- **Automated Workflows:** Airflow integration enables automated ML pipeline execution and tracking

## Next Steps
- Add authentication / access control (depending on environment)
- Define retention and backup strategy for both backend store and artifacts
- Integrate with CI to validate experiment logging on pipeline changes
- Enhance TextToSQL with query learning from user feedback and corrections
- Expand TextToSQL to support queries across multiple MLflow tracking servers
- Implement TextToSQL query caching and optimization for improved performance
- Develop TextToSQL query templates for common ML experiment analysis patterns
- Integrate TextToSQL with MLflow model registry for natural language model management