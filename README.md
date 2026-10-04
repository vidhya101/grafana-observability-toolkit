# Grafana Observability Toolkit

25 production-grade Grafana dashboards for Kubernetes/DevOps/SRE observability, plus a
comprehensive, searchable PromQL & LogQL query reference — built and battle-tested against a
real multi-cluster kubeadm lab (metrics via Prometheus, logs via Loki, traces via Jaeger, alerts
via Alertmanager).

Everything here is stack-agnostic: point it at any Prometheus + kube-state-metrics setup (Loki,
Alertmanager, and Jaeger are optional extras a few dashboards use) and it works — no dependency on
how the lab this was built against is provisioned.

## What's in here

- **`dashboards/`** — 25 dashboard JSON files, ready to import into Grafana.
- **`reference/promql-logql-query-reference.html`** — a single self-contained HTML page: ~150+
  classified PromQL/LogQL queries across SRE/DevOps/Platform/Cloud/MLOps/AIOps scenarios, with
  severity color-coding, sortable/filterable columns, and a beginner walkthrough of how to read a
  query and its output. Open it directly in any browser — no server needed.

## Dashboards

| Dashboard | What it covers |
|---|---|
| Cluster Overview | Node readiness, pod counts, PVC/service health, CPU & memory headroom |
| Node Health | Node conditions (Ready/DiskPressure/MemoryPressure/PIDPressure), per-node CPU/mem |
| Namespace Overview | ResourceQuota/LimitRange usage, per-namespace CPU/pod counts |
| Namespace / Workload Health | Deployment/StatefulSet/DaemonSet rollout health per namespace |
| Deployment & Autoscaling Health | Rollout status, HPA scaling state |
| Pod / Deployment / Service Explorer (3) | Drill-down views keyed by a namespace/name picker |
| Cluster Object Inventory | Counts of every major K8s object type, cluster-wide |
| DNS & CNI Health | CoreDNS error rates, Calico/CNI agent health |
| Storage Overview | PVC/PV phase, StorageClass defaults |
| Ingress Traffic | Request rate, status-code breakdown, 4xx/5xx trends |
| API Server & Security | apiserver latency percentiles, RBAC 403/401 rates |
| Alerts Overview | Firing Alertmanager alerts by severity |
| Golden Signals — Best Practice Overview | RED + USE + Google golden signals in one view |
| Incident Cockpit | One dynamic dashboard covering 150+ diagnostic scenarios via a dropdown, plus live per-host CPU/mem/disk/load/process/network panels and a severity-filtered live log tail |
| Server Fleet Health | Golden-signal fleet view modeled on a production dashboard design guide |
| P1 Incident / Deployment-Crash RCA / Peak Traffic / Gray Day | Four incident-response dashboards for common on-call scenarios (initial triage, crash/deploy root-cause, traffic-spike capacity, "nothing's on fire but something's off" days) |
| ArgoCD / Istio Service Mesh / Vault | Platform-component dashboards for the GitOps, service-mesh, and secrets-management layers |

Several dashboards (Cluster Overview, Node Health, Golden Signals, Fleet Health, Namespace
Overview, Incident Cockpit) carry a `cluster` template variable wired into every panel query, so
if you run more than one Prometheus-scraped cluster into a single Grafana, you get a live picker
to filter to just one — the rest ship with the picker too but need the same wiring if you want it.

## Prerequisites

- **Grafana** 9+ (uses standard panel types only — Stat, Timeseries, Table, Logs, Bar gauge)
- **Prometheus** with **kube-state-metrics** and **node_exporter** scraped (required — most panels
  depend on `kube_*` and `node_*` metrics)
- **Loki** (optional — powers the log panels; dashboards degrade gracefully without it)
- **Alertmanager** (optional — powers the Alerts Overview panels)
- **Jaeger** (optional — only referenced by name in a couple of platform panels)
- A **process-exporter** or equivalent, if you want the "top processes by CPU/memory" panels in
  Incident Cockpit / Fleet Health to populate

## Importing the dashboards

1. In Grafana: **Dashboards → New → Import**.
2. Upload a JSON file from `dashboards/`, or paste its contents.
3. Grafana will prompt you to map each dashboard's datasource inputs (Prometheus, and Loki/
   Alertmanager/Jaeger where used) to the actual datasources configured in your instance — pick
   yours and import.
4. Repeat per dashboard, or script it with the
   [Grafana HTTP API](https://grafana.com/docs/grafana/latest/developers/http_api/dashboard/)
   (`POST /api/dashboards/db`) if you're importing all 25 at once.

Metric names throughout assume the standard `kube-state-metrics` + `node_exporter` label
conventions — if your setup relabels things differently, you'll need to adjust panel queries
accordingly.

## Using the query reference

Just open `reference/promql-logql-query-reference.html` in a browser, or host it anywhere static
files are served (GitHub Pages, S3, etc.). It's fully self-contained — no build step, no external
dependencies, dark/light theme aware.

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, adapt it, ship it.

---

### Who maintains this

[RRR Solution Providers](https://www.rrrsolutionproviders.ca?utm_source=github&utm_medium=readme&utm_campaign=repos&utm_content=grafana-observability-toolkit) — cloud, Kubernetes and platform
engineering, Toronto, Canada.

We publish our prices, which is unusual in consulting: [www.rrrsolutionproviders.ca/pricing](https://www.rrrsolutionproviders.ca/pricing?utm_source=github&utm_medium=readme&utm_campaign=repos&utm_content=grafana-observability-toolkit).
If you want something like this built properly in your own estate, the smallest
way to start is a [five-day fixed-price audit](https://www.rrrsolutionproviders.ca/audit?utm_source=github&utm_medium=readme&utm_campaign=repos&utm_content=grafana-observability-toolkit) — read-only access, and you
keep the written report whether or not you continue with us.

Issues and corrections are welcome.
