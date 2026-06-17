# Data Backup System

A comprehensive Java backup system that serializes objects to disk, restores
them, manages versioned history, and supports multiple file formats —
binary, GZIP-compressed binary, CSV, and JSON — alongside file comparison,
merging, scheduling, and recovery point management.

## Project Goals

This project demonstrates a complete, self-contained backup and recovery
pipeline built entirely on the Java standard library (plus a small
dependency-free JSON layer, see note below). The goals are to:

- Serialize arbitrary key/value backup data to multiple formats
- Support round-trip import/export for CSV and JSON
- Track multiple historical versions of backups and let a user inspect or
  roll back to any of them
- Compress backups to save disk space and report compression ratios
- Compare two backups and merge them with configurable conflict resolution
- Register named recovery points that bundle all format variants of a
  backup together
- Run backups automatically on a recurring schedule
- Clean up old backup files past a retention window

## Project Structure

```
backup-project/
├── README.md
├── src/com/backup/
│   ├── Main.java                      Console entry point / menu
│   ├── BackupManager.java             Central coordinator
│   ├── data/
│   │   ├── BackupData.java            Serializable backup snapshot
│   │   └── RecoveryPoint.java         Named pointer to a backup's files
│   ├── io/
│   │   ├── BinarySerializer.java      Plain Java serialization
│   │   ├── CSVHandler.java            CSV import/export
│   │   ├── JSONHandler.java           JSON import/export
│   │   ├── FileComparator.java        Diff and merge utilities
│   │   └── FileUtils.java             Directory/size/checksum helpers
│   ├── compression/
│   │   └── CompressionUtils.java      GZIP compression
│   └── version/
│       ├── VersionManager.java        Version history tracking
│       ├── RecoveryPointManager.java  Recovery point tracking
│       └── BackupScheduler.java       Automatic scheduled backups
├── backups/                           Created at runtime (sample outputs)
├── docs/                              Format specs, architecture, testing
└── examples/                          Example CSV/JSON backup configs
```

## Setup Instructions

### Prerequisites

- Java Development Kit (JDK) 11 or later
- No external libraries are required to build or run this project

### Build

From the project root:

```bash
javac -d out $(find src -name "*.java")
```

This compiles every source file into the `out/` directory, preserving the
package structure (`out/com/backup/...`).

### Run

```bash
java -cp out com.backup.Main
```

You'll see the interactive menu:

```
=== DATA BACKUP SYSTEM ===
1. Create New Backup
2. Restore from Binary
3. Restore from Compressed
4. Import from CSV
5. List All Versions
6. Cleanup Old Backups
7. Backup Statistics
8. Compare Two Backups
9. List Recovery Points
10. Exit
Enter your choice:
```

Backups are written to a `backups/` folder created next to wherever you run
the program from.

## A Note on JSON Library Choice

The original brief suggests using Jackson or Gson for JSON serialization.
`JSONHandler.java` instead ships a small, dependency-free JSON reader/writer
written against the same flat data shape used everywhere else in this
project (a metadata header plus a string/number/boolean key-value map).

This keeps the project buildable with zero external downloads. If you do
have Gson available in your own build environment, swapping it in is
mechanical — see the "Swapping in Gson/Jackson" section in
`docs/architecture.md`.

## Quality Checklist

- [x] Project overview and goals (this file)
- [x] Setup and run instructions (this file)
- [x] Code structure with clear package hierarchy (`src/com/backup/...`)
- [x] Technical architecture and algorithm explanation (`docs/architecture.md`)
- [x] File format specifications (`docs/file-formats.md`)
- [x] Recovery procedures and backup strategy (`docs/recovery-procedures.md`)
- [x] Testing evidence and example walkthroughs (`docs/testing.md`)
- [x] Example backup configuration files (`examples/`)

## License

This is a learning project; use and adapt freely.
# Data-Backup-System
