# Learning plan: systems programming

You're not starting from zero. Treat this as **filling the gaps between application engineering and systems work**, not as learning to program again.

## 1. Decide what "systems programmer" means for you

The Let's Get Rusty advice that matters most: **pick a lane and stay there**. Systems is huge (kernels, databases, networking, runtimes, embedded, security tooling). Aimlessness is the main failure mode.

Given a background in application development, server provisioning, security courses, and small hardware projects, three natural tracks:

| Track | Why it fits | Example end project |
|---|---|---|
| **Storage / databases** | You've run DB servers; durability & indexing click faster | Persistent KV store → mini SQL engine |
| **Networking / infra** | You've provisioned web servers | TCP proxy, HTTP/1.1 server, or load balancer |
| **Security-adjacent systems** | Security courses + systems curiosity | Sandbox, packet filter, or crypto-adjacent tool (careful scope) |

Pick **one primary track** for 6–12 months. Hardware/embedded can stay a side interest; don't make it the main path unless that's the job you want.

Also decide the outcome: **career shift**, **deeper craft**, or **hobby depth**. That changes how "portfolio-grade" your projects need to be.

## 2. Use a three-layer model (not "learn Rust, then somehow systems")

### Layer A — Mental models (parallel with coding, not before forever)

You need fluent intuition for things app work usually abstracts away:

- Process vs thread vs async task
- Virtual memory, stack/heap, page faults
- Syscalls, file descriptors, blocking vs non-blocking I/O
- TCP/IP at the "what does `read`/`write` actually do" level
- Consistency, durability, crash recovery (especially if storage track)

Good anchors: *Operating Systems: Three Easy Pieces* (free), Beej's Guide to Network Programming, and for storage: simple Bitcask/LSM writeups.

Don't binge textbooks. Read a chapter, then **force it into a small program**.

### Layer B — Rust as the systems tool

As a senior engineer: skip toy tutorials aimed at juniors. Aim for fluency in the 20% that systems code lives in:

- Ownership, borrowing, lifetimes (enough to stop fighting the compiler)
- `Result`/`Option`, custom errors
- Traits, generics, iterators
- `Arc`/`Mutex`/`RwLock`, channels, threads
- `std::fs`, `std::net`, buffers (`Vec`/`Bytes`), serialization
- Later: `unsafe`, FFI, atomics, async runtimes — **only when a project demands them**

Resources that match this level: [The Rust Book](https://doc.rust-lang.org/book/) (skim, don't grind), then [PingCAP Talent Plan (Rust)](https://github.com/pingcap/talent-plan) — it's literally "build a networked KV store" and maps well to systems hiring signals.

### Layer C — One serious project, deepened over time

Junior portfolio pieces (CLI todo, basic REST API) won't teach systems thinking and won't impress anyone in 2026+. Prefer **one project that grows**.

Example ladder for a **storage** track:

1. In-memory KV with a clean CLI + tests
2. Append-only log on disk + crash recovery
3. Compaction / index rebuild
4. Sync TCP client/server protocol
5. Thread pool + concurrent reads
6. Benchmarks vs a real store (`sled` / RocksDB bindings)
7. Write-up of tradeoffs (this is the "senior" signal)

Same idea for networking: raw TCP echo → framing → HTTP/1.1 subset → connection pooling → metrics/backpressure.

## 3. How a senior should study (different from juniors)

**Timebox scope creep.** When you want to rewrite the architecture or chase a new YouTube rabbit hole, write it in a `PARKING.md` and finish the current milestone.

**Understand before generating.** Use AI for boilerplate and "remind me of the API," not for the core design of buffers, locking, or recovery. If you can't explain why a `Mutex` or WAL design is correct, you didn't learn it.

**Debug with systems tools early:** `strace`/`dtruss`, `lsof`, Wireshark/tcpdump, Instruments/`perf`, heap/flame graphs. That's half the job.

**Read production code in your track** after you have a working v1: bits of Redis, LevelDB, tokio, or a small Rust systems crate. Reading becomes useful once you've felt the pain yourself.

**Document tradeoffs in this repo.** Short ADRs ("why append-only log before B-tree") matter more than polished README badges.

## 4. A concrete 12-week starter plan

Assuming ~6–10 focused hours/week and a storage-leaning track:

| Weeks | Focus | Done when |
|---|---|---|
| 1–2 | Rust fluency + OS/memory refresh | Small programs; you can explain ownership & virtual memory without notes |
| 3–4 | In-memory KV + tests + CLI | Clean API; no "it compiles so it's done" |
| 5–7 | Persistence + WAL + recovery | Kill -9 mid-write, restart, data consistent |
| 8–9 | Network protocol (sync) | Separate client/server binaries |
| 10–11 | Concurrency + benchmarks | Numbers, not vibes; known bottlenecks |
| 12 | Write-up + next milestone | Public notes on design; pick compaction, async, or SQL subset |

If networking fits better, swap weeks 3–7 for sockets → protocol framing → HTTP subset, and keep the same "deepen one system" discipline.

## 5. How to use this repo

Keep it as a **lab notebook**, not a graveyard of half-started clones:

- One main project directory
- `notes/` for OSTEP / networking / Rust concepts tied to *what you just built*
- Milestones as tags or short changelog entries
- A living `ROADMAP.md` with the track you chose and what's explicitly out of scope

## 6. What to optimize for first

1. **Pick the track** (storage vs networking vs security tooling).
2. **Start the Talent Plan–style KV path** *or* a networked proxy — not both.
3. **Schedule systems reading as accompaniment**, not a prerequisite semester.
4. **Measure and break things** (crash, overload, concurrent clients) — that's the systems muscle.

## Decisions still open

- [ ] Outcome: career shift / deeper craft / hobby depth
- [ ] Primary track: storage / networking / security-adjacent
- [ ] First project: Talent Plan–style KV / networked proxy / other
