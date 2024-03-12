# DbTune Benchmark

This is a containerized version of the synthetic workload in the 
[DBTune Guide]. This is a postgres-14 container together with the benchbase
dependencies.

Given that it's containerized, this has the following limitations compared to
the native benchmark.

*   The container has not been setup to accept dbtune on the host. Thus, to 
    enable the restart parameters may need more work.
*   DBTune takes the machine specifications without allowing the users to
    override it. This means that on containers, the specifications won't match
    the container limits.
*   DBTune cannot determine if the disk is an `ssd` or `hhd` due to the volume.
    One needs to patch the DBTune source code to return the right value.

The source codes under `db/synthetic_workload` were taken from the 
[Synthetic Workload Repo]. This repo does not claim ownership or copyright for
those files.

[DBTune Guide]: https://github.com/dbtuneai/synthetic_workload/wiki/DBtune-synthetic-workload-guide
[Synthetic Workload Repo]: https://github.com/dbtuneai/synthetic_workload