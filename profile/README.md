<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="atsu-dark.svg">
    <img alt="atsu" src="atsu-light.svg" width="240">
  </picture>
</p>

<p align="center"><b>I/O observability for compute-intensive infrastructure.</b><br>
Seattle, 2018–2020 · archived · revived 2026 on eBPF</p>

---

atsu built per-process I/O visibility for HPC and rendering farms: a host agent captured every NFS/SMB and block I/O, streamed it through Kafka, and ML models predicted job runtimes and flagged the jobs hurting shared storage.

The company is no longer operating. The code is kept here as an archive and is being revived as a personal project — the RHEL7 kernel module is replaced by eBPF tracepoints, and the stack runs on a home Kubernetes cluster.

### How it fits together

```
eBPF tracer (nfs:*, block:*)  ─┐
legacy gather + atsu.ko        ─┴─► Kafka ─┬─► summarizer ─► gator
                                          ├─► health ─► chatops (Slack)
                                          ├─► queuepred (runtime prediction)
                                          └─► logstash ─► Elasticsearch ─► Kibana / view
```

### Public repositories

| Repo | What |
|------|------|
| [traceout](https://github.com/atsu/traceout) | Go library for Linux ftrace (fork of google/traceout) |
| [cprepo](https://github.com/atsu/cprepo) | GitHub Action: copy a file into another repo via PR *(archived)* |
| [k8slynter](https://github.com/atsu/k8slynter) | GitHub Action for Kubernetes YAML linting *(archived)* |
| [rpmbuilder](https://github.com/atsu/rpmbuilder) | Docker image for building RPMs *(archived)* |

Core platform repos (`gather`, `kernel`, `goat`, `gator`, `health`, `queuepred`, …) are private.
