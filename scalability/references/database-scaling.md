# Database Scaling Patterns

## Connection Pooling

### PgBouncer (PostgreSQL)
```ini
# pgbouncer.ini
[databases]
mydb = host=postgres port=5432 dbname=mydb

[pgbouncer]
listen_port = 6432
listen_addr = *
auth_type = md5
auth_file = /etc/pgbouncer/userlist.txt
pool_mode = transaction    # Best for APIs (release connection after each transaction)
max_client_conn = 1000     # Max client connections to PgBouncer
default_pool_size = 20     # Connections to PostgreSQL per database+user pair
reserve_pool_size = 5      # Emergency extra connections
server_idle_timeout = 600  # Close idle server connections after 10min
```

Pool modes:
- `session` — connection held for entire client session (expensive)
- `transaction` — connection released after each transaction (recommended for APIs)
- `statement` — released after each statement (only for simple queries)

### SQLAlchemy Connection Pool
```python
from sqlalchemy import create_engine
from sqlalchemy.pool import QueuePool
import os

engine = create_engine(
    os.environ["DATABASE_URL"],
    pool_size=20,           # maintained connections
    max_overflow=10,        # extra connections under load
    pool_pre_ping=True,     # test connection health before use
    pool_recycle=3600,      # recycle connections after 1 hour
    pool_timeout=30,        # wait timeout for connection
    poolclass=QueuePool
)
```

---

## Read Replicas

### When to Add a Read Replica
- Read/write ratio > 80% reads
- Read queries causing lock contention on primary
- Reporting/analytics queries slowing down production

### Django with Read Replica
```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'HOST': os.environ['DB_PRIMARY_HOST'],
        'NAME': 'mydb',
    },
    'replica': {
        'ENGINE': 'django.db.backends.postgresql',
        'HOST': os.environ['DB_REPLICA_HOST'],
        'NAME': 'mydb',
        'TEST': {'MIRROR': 'default'},
    }
}

DATABASE_ROUTERS = ['myapp.db_routers.PrimaryReplicaRouter']

# db_routers.py
class PrimaryReplicaRouter:
    def db_for_read(self, model, **hints):
        return 'replica'

    def db_for_write(self, model, **hints):
        return 'default'

    def allow_relation(self, obj1, obj2, **hints):
        return True

    def allow_migrate(self, db, app_label, model_name=None, **hints):
        return db == 'default'
```

### SQLAlchemy with Read Replica
```python
from sqlalchemy.orm import Session

def get_db_read():
    """Read replica session."""
    db = SessionLocalReplica()
    try:
        yield db
    finally:
        db.close()

def get_db_write():
    """Primary session for writes."""
    db = SessionLocalPrimary()
    try:
        yield db
    finally:
        db.close()

# FastAPI
@app.get("/users/{id}")
async def get_user(id: int, db: Session = Depends(get_db_read)):
    return db.query(User).filter(User.id == id).first()

@app.post("/users")
async def create_user(data: UserCreate, db: Session = Depends(get_db_write)):
    user = User(**data.dict())
    db.add(user)
    db.commit()
    return user
```

---

## Indexes

### Index Types (PostgreSQL)
```sql
-- B-tree: default, for =, <, >, BETWEEN, IN
CREATE INDEX idx_users_email ON users(email);

-- Partial index: only index a subset
CREATE INDEX idx_active_users ON users(email) WHERE active = true;

-- Composite index: column order matters!
-- Put high-cardinality columns first (unless filtering by second column alone)
CREATE INDEX idx_pipeline_runs ON pipeline_runs(status, created_at DESC);

-- Covering index: include all columns query needs
CREATE INDEX idx_videos_niche ON agent_outputs(niche) INCLUDE (quality_score, created_at);

-- GIN index: for JSONB, arrays, full-text search
CREATE INDEX idx_metadata ON agent_outputs USING GIN(metadata);
```

### Finding Missing Indexes
```sql
-- Find slow queries (pg_stat_statements extension required)
SELECT query, calls, mean_exec_time, total_exec_time
FROM pg_stat_statements
ORDER BY total_exec_time DESC
LIMIT 20;

-- Find unused indexes (candidates for removal)
SELECT indexrelid::regclass AS index, relid::regclass AS table,
       idx_scan, idx_tup_read, idx_tup_fetch
FROM pg_stat_user_indexes
WHERE idx_scan = 0
ORDER BY pg_relation_size(indexrelid) DESC;

-- Find sequential scans on large tables (missing indexes)
SELECT relname, seq_scan, seq_tup_read, idx_scan
FROM pg_stat_user_tables
WHERE seq_scan > 0
ORDER BY seq_tup_read DESC;
```

---

## Supabase-Specific Scaling (AI Agency)

### pgvector Index for RAG
```sql
-- For Customer Support Agent RAG embeddings
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE knowledge_base (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    content TEXT NOT NULL,
    embedding vector(1536),  -- OpenAI ada-002 dimensions
    metadata JSONB,
    created_at TIMESTAMPTZ DEFAULT now()
);

-- IVFFlat index for approximate nearest neighbor search
-- Build after inserting ~1000 rows; lists = sqrt(rows)
CREATE INDEX ON knowledge_base
USING ivfflat (embedding vector_cosine_ops)
WITH (lists = 100);

-- Query: find similar documents
SELECT content, metadata,
       1 - (embedding <=> query_embedding) AS similarity
FROM knowledge_base
ORDER BY embedding <=> query_embedding
LIMIT 5;
```

### Row Level Security (Performance Impact)
```sql
-- RLS adds overhead -- use policies that can use indexes
CREATE POLICY "user_sees_own_data" ON agent_outputs
    FOR SELECT USING (user_id = auth.uid());

-- Check: explain analyze should use index, not seq scan
EXPLAIN ANALYZE
SELECT * FROM agent_outputs WHERE user_id = 'xxx';
```
