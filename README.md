# hamsterscan

Shows requests, limits, and actual usage for all pods in a namespace — in one table, with column totals.

Joins `oc get pods` (spec) with `oc adm top pods` (live metrics) **by pod name**, so rows always match even when the two commands sort differently.

## Usage

```bash
./hamsterscan <namespace>
```

## Example output

```
NAME            CPU_REQ   MEM_REQ      CPU_LIM    MEM_LIM      CPU_ACTUAL  MEM_ACTUAL
normplugin-0    50m,100m  128Mi,512Mi  500m,200m  512Mi,768Mi  1m          353Mi
normplugin-1    50m,100m  128Mi,512Mi  500m,200m  512Mi,768Mi  1m          354Mi
-----
SUMME           15.50     39.0Gi       3.50       71.8Gi       489m        13.0Gi
```

Comma-separated values = one value per container in the pod. The totals handle these correctly, normalize units (m, Mi, Gi, Ki), and count `<none>` as 0.

## Requirements

- `oc` with cluster access
- Metrics API available (`oc adm top` must work)

## Notes

- Pods without metrics (just started, completed) are dropped by the join and not counted
- Read-only, nothing gets modified
