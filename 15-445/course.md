# 15-445/645: Intro to Database Systems

CMU's flagship DBMS course. I used the Fall 2024 archive as the source of truth here because the public playlist is labeled Fall 2024 and points back to that offering.

## Quick Facts

- **Course:** 15-445/645 Intro to Database Systems
- **School:** Carnegie Mellon University
- **Offering used for this KB page:** Fall 2024 archive
- **Instructor:** Andy Pavlo
- **Meeting time:** Mon/Wed, 2:00pm-3:20pm ET
- **Location:** Tepper 1403
- **Prerequisite:** 15-213/513
- **Textbook:** DB System Concepts, 7th edition
- **Grading:** 15% homeworks, 45% programming projects, 20% midterm, 20% final
- **Projects:** cumulative BusTub implementations, with C++17 as a prerequisite
- **Public video source:** [CMU Intro to Database Systems (15-445/645 - Fall 2024)](https://www.youtube.com/playlist?list=PLSE8ODhjZXjYDBpQnSymaectKjxCy6BYq)
- **Official course page:** [CMU 15-445/645 Fall 2024](https://15445.courses.cs.cmu.edu/fall2024/)
- **Syllabus:** [Fall 2024 syllabus](https://15445.courses.cs.cmu.edu/fall2024/syllabus.html)
- **Schedule:** [Fall 2024 schedule](https://15445.courses.cs.cmu.edu/fall2024/schedule.html)
- **Assignments:** [Fall 2024 assignments](https://15445.courses.cs.cmu.edu/fall2024/assignments.html)
- **BusTub repo:** [cmu-db/bustub](https://github.com/cmu-db/bustub)

## Syllabus

### Lecture Arc

The archived Fall 2024 schedule walks through the full DBMS stack in order:

- Course overview and logistics
- Relational model and algebra
- Modern SQL
- Storage internals: storage I, storage II, memory management
- Physical layout: storage models and compression
- Hash tables
- Indexes and filters I, II
- Index concurrency control
- Query processing: sorting and aggregation, joins, execution I, execution II
- Query planning and optimization
- Transaction processing: concurrency control theory, two-phase locking, timestamp ordering, MVCC
- Durability: database logging and recovery
- Distributed systems: introduction to distributed databases, distributed OLTP, distributed OLAP
- Final review and systems potpourri

The public lecture videos are interleaved with flash talks from practitioners at Neon, StarTree, RelationalAI, TiDB, dbt Labs, ClickHouse, Firebolt, Weaviate, Confluent, and DataStax.

### Coursework

- **Homeworks:** SQL, Storage, Indexes & Filters, Query Execution, Concurrency Control, Distributed Databases
- **Projects:** C++ Primer, Buffer Pool Manager, Database Index, Query Execution, Concurrency Control
- **Shape of the work:** homeworks reinforce the lectures; projects are cumulative and all run inside BusTub
- **Exams:** one midterm and one final

## Instructor Bios

### Andy Pavlo

Associate Professor of Databaseology in Carnegie Mellon University's Computer Science Department. His research centers on database management systems, especially self-driving and autonomous architectures, transaction processing systems, and large-scale data analytics. He is part of the CMU Database Group and the Parallel Data Laboratory.

## Links

- [Course home](https://15445.courses.cs.cmu.edu/fall2024/)
- [Syllabus](https://15445.courses.cs.cmu.edu/fall2024/syllabus.html)
- [Schedule](https://15445.courses.cs.cmu.edu/fall2024/schedule.html)
- [Assignments](https://15445.courses.cs.cmu.edu/fall2024/assignments.html)
- [Public playlist](https://www.youtube.com/playlist?list=PLSE8ODhjZXjYDBpQnSymaectKjxCy6BYq)
- [Andy Pavlo faculty page](https://www.csd.cs.cmu.edu/people/faculty/andrew-pavlo)
- [Andy Pavlo personal page](https://www.cs.cmu.edu/~pavlo/)
- [BusTub repo](https://github.com/cmu-db/bustub)
- [DB System Concepts, 7th ed.](https://www.db-book.com/)

## Why It Matters

Inference from the syllabus:

- It is a clean pass through the whole database engine stack: data model, storage layout, indexing, execution, concurrency, recovery, and distributed query processing.
- It forces the theory to become code. The BusTub projects are the point, not a side quest.
- It explains the exact primitives that modern data infrastructure still depends on: row/column formats, B+ trees, hash tables, query optimizers, transaction protocols, and OLTP/OLAP tradeoffs.
- It stays close to real systems through case studies and flash talks, so the abstractions do not drift too far from shipped software.
- If you want one course that explains why a database behaves the way it does under load, this is the one.

## Sources

- [CMU 15-445/645 Fall 2024 home page](https://15445.courses.cs.cmu.edu/fall2024/)
- [CMU 15-445/645 Fall 2024 syllabus](https://15445.courses.cs.cmu.edu/fall2024/syllabus.html)
- [CMU 15-445/645 Fall 2024 schedule](https://15445.courses.cs.cmu.edu/fall2024/schedule.html)
- [CMU 15-445/645 Fall 2024 assignments](https://15445.courses.cs.cmu.edu/fall2024/assignments.html)
- [Andy Pavlo faculty page](https://www.csd.cs.cmu.edu/people/faculty/andrew-pavlo)
- [Andy Pavlo personal page](https://www.cs.cmu.edu/~pavlo/)
- [cmu-db/bustub](https://github.com/cmu-db/bustub)
- Public playlist metadata verified locally on 2026-04-02 with:
  - `yt-dlp -J --flat-playlist --skip-download 'https://www.youtube.com/playlist?list=PLSE8ODhjZXjYDBpQnSymaectKjxCy6BYq' | jq '{title,description,uploader,uploader_id,channel,playlist_count:(.entries|length),webpage_url}'`
  - Output: `CMU Intro to Database Systems (15-445/645 - Fall 2024)`, `playlist_count: 26`
