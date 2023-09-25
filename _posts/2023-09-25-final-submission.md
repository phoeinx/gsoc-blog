---
title: "Final Submission and Overview of my GSoC'23 Project at MariaDB ColumnStore"
date: 2023-09-25
---

## 1. Project Summary
During my project I optimized the `GROUP BY` operator in `MariaDB ColumnStore` for workloads requiring disk-based aggregation. To achieve this I *a)* reduced unnecessary I/O operations in the aggregation algorithm, *b)* fixed bugs in the previous implementation of disk-based aggregation and *c)* tested vectorized access to the internal hash map used for aggregation.
Additionally, I fixed other bugs and improved documentation and the development setup, especially for new developers.

## 2. What I did

### 2.1 Documenting

- **Updating BUILD.md** ([PR](https://github.com/mariadb-corporation/mariadb-columnstore-engine/pull/2875/files)): Following my own first setup process, I used the fresh point of view to updated the build instructions and streamline the setup information available in the MariaDB ColumnStore (MCS) repository.
- **Creating DEVELOPING.md** ([PR](https://github.com/mariadb-corporation/mariadb-columnstore-engine/pull/2955)): Throughout my learning process about the development and testing process in MCS I documented my findings to hopefully help future new contributors.
- **Vagrant Setup** ([PR](https://github.com/mariadb-corporation/mariadb-columnstore-engine/pull/2955)): To facilitate local developing on non-Linux systems (e.g. for Mac-Users), I created a Vagrantfile (and worked with it until development ran smoothly.) The Vagrantfile also includes provisioning and enables hitting the floor running when starting to work on MCS (and not having to sort out dependencies of a local machine etc.).

### 2.2 Developing

- **Issues MCOL-4632 and MCOL-4648** ([PR](https://github.com/mariadb-corporation/mariadb-columnstore-engine/pull/2874)): Closed open issues (and further related issues I've found during fixing them) dealing with `CAST()` operations leading to invalid values. And learnt quite a bit about backwards compatibility. 
- **Disk-based Aggregation Speed-up** ([PR](https://github.com/mariadb-corporation/mariadb-columnstore-engine/pull/2957/files)): Reduced I/O operations for `GROUP BY` operations (without `DISTINCT` including aggregations), by deleting a whole pass over the data. (Which included reading most data from disk and then writing it again.)
- **Disk-based Aggregation Output Fix** ([PR](https://github.com/mariadb-corporation/mariadb-columnstore-engine/pull/2957/files)): Found cause for bug which caused MCS to output only a small part of disk-based aggregation results. Implemented fix to include all aggregated results in output. Identified further issue with disk-based aggregation for queries containing an aggregation including a `DISTINCT` operation, (e.g. `SUM(DISTINCT age)`). 

### 2.3 Designing/Researching


## 3. Current State & What's left to Do
Section 4 shows the current status of my PRs. My work on the casting issues and documentation is merged and I'll continue work on the disk-based aggregation PRs to see them merged.

## 4. Opened and Merged PRs

| PR Title  | Merge Status |
| -------- | ------- |
| [Fix MCOL 4632 and 4648 and further issues when casting to [U]BIGINT.](https://github.com/mariadb-corporation/mariadb-columnstore-engine/pull/2874)  |  Merged  |
| [Update BUILD.md.](https://github.com/mariadb-corporation/mariadb-columnstore-engine/pull/2875) | Merged     |
| [Add DEVELOPING.md and Vagrantfile to improve developing documentation and flow.](https://github.com/mariadb-corporation/mariadb-columnstore-engine/pull/2955)    | Merged    |
| [Optimize TupleAggregateStep::doThreadedAggregate() through removing unnecessary I/O.](https://github.com/mariadb-corporation/mariadb-columnstore-engine/pull/2957/files) (At this moment contains both the disk-based aggregation I/O fix and the I/O optimization.)|   Open       |


## 5. Key Learnings
- 

## 6. Acknowledgements

First of all  I want to thank my mentor Roman (@drruty) for making a GSoC project fitting to my skills and experience possible for me. I also want to thank him for his guidance, patience and good humor throughout the project. 

A big thank you also the the MariaDB foundation for hosting GSoC projects like mine and enabling me to visit the MariaDB Unconference in October.

Last, I want to thank Google for creating and organizing GSoC. It's a great program and I've been very happy to participate. 