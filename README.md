
# 🧠 Caching DNS Server

## 📌 Project Overview

This project implements a **caching DNS server** that listens on **port 53** and handles **recursive DNS queries**. It is designed for robustness, efficiency, and cache persistence between runs.

## 🛠 Features

- ✅ **Recursive Query Handling**: Accepts DNS requests from clients and performs full resolution.
- 📦 **Full Response Parsing**: Parses the entire DNS response, including `Answer`, `Authority`, and `Additional` sections.
- 🗃 **Intelligent Caching**:
  - Caches *all* useful resource records (RRs), not just the answer to the client’s question.
  - Maintains two main mappings:
    - **Domain Name → IP Address**
    - **IP Address → Domain Name**
- ⏳ **TTL-Based Cache Expiration**: Periodically checks the cache and removes expired entries based on their TTL values.
- 💾 **Cache Persistence**:
  - On shutdown, the cache is **serialized and saved to disk**.
  - On startup, the server **deserializes the cache**, removes expired entries, and restores valid ones into memory.
- 🛡 **Fault Tolerance**:
  - Gracefully handles cases where upstream DNS servers are unresponsive (e.g., no infinite waits, no crashes).

## 📚 Supported DNS Record Types

The server currently supports parsing and caching the following record types:
- `A` – IPv4 addresses
- `AAAA` – IPv6 addresses
- `NS` – Name servers
- `PTR` – Reverse DNS pointers

## 🚀 How It Works

1. **Startup**:
   - Loads previously cached data from disk (if available).
   - Removes any records whose TTLs have expired.
2. **Query Processing**:
   - Listens on port 53 for client requests.
   - If a requested record is **not in cache**, it performs a **recursive lookup**.
   - Parses and stores *all* resource records from the DNS response.
3. **Caching**:
   - Stores RRs in hash maps for quick access.
   - Periodically checks and removes stale entries based on TTLs.
4. **Shutdown**:
   - Saves the current state of the cache to disk for reuse.

## 📂 File Structure

> _To be customized based on your actual file layout._

```bash
dns-server/
├── cache/
│   └── cache_store.dat      # Serialized cache file
├── dns/
│   ├── dns_server.py        # Main server code
│   └── parser.py            # DNS response parser
├── utils/
│   └── cache_utils.py       # TTL check, serialization/deserialization
├── logs/
│   └── server.log           # Runtime logs
└── README.md
```

## ⚙ Requirements

- Python 3.x
- `socket` and `struct` libraries
- (Optional) `pickle` or `json` for serialization

## 🧪 Testing and Running

To run the DNS server:
```bash
sudo python3 dns_server.py
```

> Use `sudo` because binding to port 53 requires administrative privileges.

You can test the server using tools like:
- `dig @127.0.0.1 example.com`
- `nslookup example.com 127.0.0.1`

---

Let me know if you'd like to add command-line arguments, configuration options, or contributor instructions too!
