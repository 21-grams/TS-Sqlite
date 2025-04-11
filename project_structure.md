# Project Structure

```
sensor-monitoring-api/
├── Cargo.toml
├── src/
│   ├── main.rs
│   ├── db/
│   │   ├── mod.rs
│   │   ├── schema.rs
│   │   └── migrations.rs
│   ├── models/
│   │   ├── mod.rs
│   │   ├── sensor.rs
│   │   ├── reading.rs
│   │   └── session.rs
│   ├── api/
│   │   ├── mod.rs
│   │   ├── sensors.rs
│   │   ├── readings.rs
│   │   └── sessions.rs
│   └── utils/
│       ├── mod.rs
│       └── error.rs
└── migrations/
    └── 001_initial_schema.sql
```

This structure organizes our Rust project into logical modules:

- `db`: Database connection and schema management
- `models`: Data models for the application
- `api`: API routes and handlers
- `utils`: Utility functions and error handling
- `migrations`: SQL migration files
