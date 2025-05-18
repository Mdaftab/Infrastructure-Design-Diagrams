# Stock Data Dashboard Architectural Approaches

Here are two potential architectural approaches for the stock data dashboard application. The diagram can be found in the `diagrame.md` file.

## End-to-End Architecture Overview

The stock data dashboard architecture represents a complete data pipeline from external stock data sources to end-user interfaces. Below is a comprehensive explanation of how all components interconnect and work together:

### 1. Stock Data Sources to Data Ingestion Service

**Connection Mechanism:**
- RESTful API calls, typically HTTPS
- WebSockets for real-time market data feeds
- FTP/SFTP for batch file downloads (less common for real-time stock data)

**Implementation Details:**
- The Data Ingestion Service implements API client libraries specific to each data source
- Connection pools manage multiple concurrent connections
- Circuit breakers prevent cascade failures if a data source becomes unavailable
- Retry mechanisms with exponential backoff handle temporary API outages
- API throttling respects rate limits of external sources

### 2. Data Ingestion Service and Pub/Sub Interaction

**Connection Mechanism:**
- Google Cloud Pub/Sub client libraries
- HTTPS for publishing messages

**Implementation Details:**
1. **Connection to Stock Data Sources**:
   - The Data Ingestion Service connects to external stock data APIs (like Yahoo Finance, Alpha Vantage, IEX Cloud, etc.)
   - It authenticates using API keys and establishes secure connections
   - It can be configured to pull data at scheduled intervals or in real-time depending on the API capabilities

2. **Data Retrieval and Initial Processing**:
   - The service requests specific stock data (prices, volumes, company information, etc.)
   - It handles rate limiting, retries, and connection failures
   - Performs initial validation to ensure data quality and completeness

3. **Publishing to Pub/Sub**:
   - The ingested data is formatted into standardized messages
   - Each message contains metadata (timestamp, source, data type) and the actual stock data payload
   - The service publishes these messages to specific topics in Pub/Sub (e.g., "stock-price-updates", "company-earnings", etc.)

4. **Pub/Sub's Role**:
   - Pub/Sub acts as a message broker that decouples the ingestion from processing
   - It maintains message queues for each topic and ensures reliable delivery
   - It can handle high throughput and scale automatically as message volume increases
   - The Data Processing Service subscribes to relevant topics
   - When new messages arrive, Pub/Sub delivers them to all subscribers
   - This enables multiple services to process the same data independently if needed

### 3. Pub/Sub to Data Processing Service

**Connection Mechanism:**
- Pub/Sub subscription pull or push model
- gRPC for high-performance communication

**Implementation Details:**
- The Data Processing Service maintains persistent subscriptions to relevant Pub/Sub topics
- Upon receiving messages, the service:
  - Deserializes message content
  - Applies business logic and transformations
  - Performs calculations (e.g., moving averages, technical indicators)
  - Enriches data with additional context (e.g., sector information, company fundamentals)
  - Aggregates data across time periods (e.g., minute/hour/day)
  - Batch processes multiple messages for efficiency
- Processing components can be scaled horizontally to handle varying loads
- Dead-letter queues capture messages that fail processing for later analysis

### 4. Data Processing Service to Time-Series Database and Cache

**Connection Mechanism:**
- Database-specific client libraries
- JDBC/ODBC connections for SQL databases
- Native API connections for NoSQL databases

**Implementation Details:**
- **Time-Series Database:**
  - Optimized for time-sequential data storage and retrieval
  - Data is partitioned by time ranges for efficient querying
  - Data is stored with appropriate retention policies (recent data at higher resolution)
  - Schema design optimizes for common query patterns (e.g., OHLCV data, technical indicators)
  - Database indices support rapid time-based and symbol-based queries
  - Implementation options include: BigTable, Cassandra, InfluxDB, or TimescaleDB

## Database Selection Rationale

For the stock dashboard application, specialized time-series databases are strongly recommended over general-purpose databases for the following reasons:

### Why Time-Series Databases

1. **Optimized for Sequential Writes and Time-Based Queries**
   - Stock data arrives as a continuous stream of time-stamped events
   - Time-series databases are optimized for appending new data points quickly
   - They provide specialized functions for time-based aggregation (OHLC, moving averages, etc.)
   - They offer efficient downsampling capabilities (e.g., converting minute data to hourly)

2. **Compression Algorithms**
   - Time-series data often contains repeating patterns and small deltas between consecutive values
   - Time-series databases implement specialized compression algorithms that can achieve 10-100x storage reduction
   - This significantly reduces storage costs and improves query performance for historical data

3. **Partitioning by Time**
   - Automatic partitioning by time windows makes it easy to manage data retention
   - Older data can be automatically moved to cheaper storage tiers
   - Queries naturally filter by time ranges, making partitioning extremely effective

4. **High Write Throughput**
   - The system needs to handle potentially thousands of data points per second during market hours
   - Time-series databases are designed for this high-frequency write pattern
   - They maintain performance even as the dataset grows to billions of data points

### Specific Database Recommendations

1. **TimescaleDB**
   - Built on PostgreSQL, providing SQL compatibility and rich query capabilities
   - Excellent for applications that need both time-series and relational data
   - Good choice if you need to store additional metadata about stocks, companies, etc.
   - Strong consistency guarantees and ACID compliance
   - Automatic partitioning of data into time-based chunks ("hypertables")

2. **InfluxDB**
   - Purpose-built for time-series data with no relational database legacy
   - Highly efficient storage and retrieval of time-stamped data
   - Built-in functionality for continuous queries, retention policies, and downsampling
   - Strong support for metrics, events, and other time-structured data
   - Tag-based indexing for fast multi-dimensional filtering

3. **Google Cloud Bigtable**
   - Massively scalable NoSQL database service
   - Excellent for high-volume applications with predictable latency requirements
   - Row keys can be designed for time-series optimization (e.g., using reversed timestamps)
   - Seamless integration with other GCP services
   - Automatic scaling with no operational overhead

4. **Apache Cassandra**
   - Distributed NoSQL database with linear scalability
   - Excellent for multi-region deployments with no single point of failure
   - Time UUID data type specifically designed for time-series data
   - Tunable consistency levels to balance availability and consistency
   - Built-in TTL feature for data expiration

### Estimated Data Volume

For a comprehensive stock dashboard application, we estimate the following data volumes:

1. **Market Data Points**
   - **US Equity Markets:**
     - ~10,000 tradable symbols
     - Tick data: ~1,000-10,000 ticks per symbol per day (active stocks)
     - OHLCV minute bars: 390 data points per symbol per day (6.5 trading hours)
     - Daily total: ~50-100 million data points for tick data, ~4 million for minute bars
   
   - **Global Coverage (additional):**
     - ~40,000 additional international symbols
     - Less frequent updates for international markets
     - Daily total: Additional ~10-20 million data points

2. **Derived Data**
   - Technical indicators (e.g., moving averages, RSI, MACD)
   - ~10-50 indicators per symbol
   - Updated at multiple timeframes (minute, hour, day)
   - Daily total: ~10-20 million additional data points

3. **Fundamental Data**
   - Quarterly financial reports
   - Company news and events
   - Analyst ratings
   - Much lower volume: ~100,000 data points per day

4. **Historical Data**
   - 10+ years of daily data for all symbols
   - 1-5 years of intraday data for active symbols
   - Total historical database size: Several terabytes

5. **Total System Scale**
   - Daily ingest: ~100-150 million data points
   - Annual growth: ~25-40 billion data points
   - Query patterns: Mixture of real-time (latest prices) and historical (charts, analysis)

This scale clearly demonstrates the need for a database solution specifically optimized for time-series data, with strong partitioning, compression, and query capabilities designed for time-based operations.

- **Cache Layer:**
  - In-memory data structure store (Redis or Memcached)
  - Stores frequently accessed data like:
    - Latest market data for popular symbols
    - Pre-calculated aggregations
    - User session data
  - Implements TTL (Time-To-Live) policies based on data freshness requirements
  - Write-through or write-behind caching strategies based on consistency needs
  - Distributed cache for horizontal scaling

### 5. Time-Series Database and Cache to Serving API

**Connection Mechanism:**
- Database client libraries
- Connection pooling for efficient resource utilization

**Implementation Details:**
- The Serving API implements repository patterns for data access
- Query optimization ensures efficient data retrieval
- Data aggregation may happen at this layer for complex visualizations
- Cached responses reduce database load for common queries
- Authorization checks ensure data access security
- Rate limiting prevents API abuse

### 6. Serving API to Frontend Dashboard

**Connection Mechanism:**
- RESTful APIs over HTTPS
- GraphQL for flexible data querying
- WebSockets for real-time updates

**Implementation Details:**
- The Serving API exposes endpoints for different data needs:
  - Historical price data retrieval
  - Real-time price updates
  - Company information and news
  - User-specific watchlists and portfolios
- API versioning ensures backward compatibility
- JWT or OAuth2 authentication secures user-specific data
- Content negotiation supports multiple formats (JSON, CSV)
- API gateway can provide additional services like:
  - Rate limiting
  - Analytics
  - Response caching
  - Request validation

### 7. Frontend Dashboard to User Browser

**Connection Mechanism:**
- HTTP/HTTPS for web content delivery
- WebSockets for real-time updates

**Implementation Details:**
- Static assets served via CDN for global performance
- Progressive Web App techniques for offline capabilities
- Responsive design for multiple device types
- Client-side state management (Redux, MobX, etc.)
- Data visualization libraries (D3.js, Highcharts, etc.) for interactive charts
- WebSocket connection management with automatic reconnection
- Client-side data caching to reduce server load

## System Behavior and Integration Points

The overall system operates with these key integration points and behaviors:

- **Real-time Updates Flow:**
  1. New market data arrives at the Data Ingestion Service
  2. Data is published to Pub/Sub
  3. Data Processing Service processes and stores in the database
  4. Updates are pushed to connected clients via WebSockets
  5. Frontend updates charts and data displays without page refresh

- **Historical Data Query Flow:**
  1. User requests historical data via the Dashboard interface
  2. Frontend makes API call to the Serving API
  3. Serving API retrieves data from Time-Series Database
  4. Data is formatted and returned to the Frontend
  5. Frontend renders visualizations and displays

- **System Scaling Behavior:**
  - Each component can scale independently based on load
  - Pub/Sub acts as a buffer during traffic spikes
  - Cache reduces database load for common queries
  - Stateless components enable easy horizontal scaling

- **Fault Tolerance:**
  - Component failures are isolated through circuit breakers
  - Data is durably stored in Pub/Sub until processed
  - Redundancy at each layer prevents single points of failure
  - Automatic failover for critical components

This architecture provides several benefits:
- Scalability: Each component can scale independently
- Reliability: Messages are persisted in Pub/Sub until successfully processed
- Loose coupling: The ingestion service doesn't need to know about downstream consumers
- Real-time capabilities: Data flows through the system with minimal latency

## Approach 1: Architecture without GKE

This approach leverages managed GCP services and/or virtual machines.

**Explanation:**
- Data is ingested from the Third-Party API by a dedicated service (VMs/Cloud Functions/Cloud Run).
- The Ingestion Service passes data to the Data Processing Service (VMs/Cloud Functions/Cloud Run/Dataflow) for cleaning and transformation.
- Processed data is stored in a chosen Data Storage solution (Cloud SQL/Spanner/Bigtable/Firestore).
- The Data Serving/API layer (VMs/Cloud Run/App Engine) retrieves data from storage and serves it to the Frontend Dashboard (Cloud Storage/CDN or App Engine/Cloud Run).
- Caching is used at the Data Serving layer for performance.
- Real-time updates are handled via WebSockets.

## Approach 2: Architecture with GKE

This approach utilizes Google Kubernetes Engine for container orchestration.

**Explanation:**
- Data Ingestion and Processing are handled by containerized microservices running in GKE (GKE Deployment/Jobs).
- Data Storage is typically a managed GCP database service outside the GKE cluster (e.g., Bigtable).
- The Data Serving/API is a containerized microservice in GKE (GKE Deployment), exposed via Kubernetes Service/Ingress.
- The Frontend Dashboard can be containerized in GKE or hosted statically (GKE Deployment or Cloud Storage/CDN).
- Caching and real-time updates are handled similarly to the non-GKE approach, potentially with caching running within GKE or as a managed service.
