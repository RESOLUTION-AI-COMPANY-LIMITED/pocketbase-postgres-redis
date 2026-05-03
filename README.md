# 🚀 PocketBase PostgreSQL & Redis

**PocketBase fork with PostgreSQL, MySQL & Redis clustering support**

Based on [postgrebase](https://github.com/zhenruyan/postgrebase) · Research docs: [postgres-migration-research](https://github.com/RESOLUTION-AI-COMPANY-LIMITED/postgres-migration-research)

---

## ✨ Features

- ✅ **PostgreSQL** support (lib/pq)
- ✅ **MySQL** support (go-sql-driver/mysql)  
- ✅ **Redis Pub/Sub** for multi-node clustering
- ✅ **Graceful degradation** (Redis optional)
- ✅ **Backward compatible** with SQLite

---

## 🚀 Quick Start

### Install

```bash
go install github.com/RESOLUTION-AI-COMPANY-LIMITED/pocketbase-postgres-redis/build@latest
```

Or build from source:
```bash
git clone https://github.com/RESOLUTION-AI-COMPANY-LIMITED/pocketbase-postgres-redis.git
cd pocketbase-postgres-redis
go build -o pocketbase ./build
```

### Run with PostgreSQL

```bash
./pocketbase serve --dataDsn="postgres://user:pass@localhost:5432/db?sslmode=disable"
```

### Run with MySQL

```bash
./pocketbase serve --dataDsn="mysql://user:pass@tcp(localhost:3306)/db"
```

### Multi-node with Redis

```bash
# Node 1
./pocketbase serve \
  --dataDsn="postgres://user:pass@shared-db:5432/db" \
  --redisDsn="redis://shared-redis:6379/0" \
  --http="0.0.0.0:8091"

# Node 2
./pocketbase serve \
  --dataDsn="postgres://user:pass@shared-db:5432/db" \
  --redisDsn="redis://shared-redis:6379/0" \
  --http="0.0.0.0:8092"
```

---

## 📖 CLI Flags

```bash
--dataDsn string         PostgreSQL/MySQL DSN
--redisDsn string        Redis DSN (optional)
--dir string             Data directory (default "pb_data")
--debug                  Enable debug mode
```

---

## 🏗️ Architecture

### Minimal Changes (~200 lines)

| Component | Lines | File |
|-----------|-------|------|
| DB connection | 29 | `core/db_postgresql.go` |
| Redis integration | ~100 | `core/base.go` |
| CLI flags | ~30 | `pocketbase.go` |

### Design Patterns

- **Factory Pattern**: Auto-detect driver from DSN
- **Graceful Degradation**: Redis optional
- **Pub/Sub**: Cross-node realtime sync

---

## 🧪 Test with Docker

```bash
# PostgreSQL
docker run -d -e POSTGRES_PASSWORD=postgres -p 5432:5432 postgres:15

./pocketbase serve --dataDsn="postgres://postgres:postgres@localhost:5432/postgres"

# Visit: http://localhost:8090/_/
```

---

## 📚 Documentation

- **Research**: https://github.com/RESOLUTION-AI-COMPANY-LIMITED/postgres-migration-research
- **PocketBase Docs**: https://pocketbase.io/docs
- **Postgrebase**: https://github.com/zhenruyan/postgrebase

---

## 🙏 Credits

- [PocketBase](https://github.com/pocketbase/pocketbase) - Original project
- [Postgrebase](https://github.com/zhenruyan/postgrebase) - PostgreSQL implementation
- [RESOLUTION AI](https://github.com/RESOLUTION-AI-COMPANY-LIMITED) - Research & enhancements

---

## 📝 License

MIT (same as PocketBase)

---

**Status**: ✅ Working · ⏳ Testing in progress
