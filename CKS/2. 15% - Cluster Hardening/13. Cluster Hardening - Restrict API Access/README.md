### Anonymous Access
- kube-apiserver --anonymous-auth=true|false

``` bash
root@cks-master:~# curl https://localhost:6443 -k
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {},
  "code": 403
}
```
/etc/kubernetes/manifests/kube-apiserver.yaml
``` yaml
...
spec:
  containers:
  - command:
    - kube-apiserver
    - --anonymous-auth=false
    - --advertise-address=10.156.0.2
...
```

``` bash
root@cks-master:~# curl https://localhost:6443 -k
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "Unauthorized",
  "reason": "Unauthorized",
  "code": 401
}
```

### Insecure Access
- kube-apiserver --insecure-port=8080

/etc/kubernetes/manifests/kube-apiserver.yaml
``` yaml
...
spec:
  containers:
  - command:
    - kube-apiserver
    - --anonymous-auth=false
    - --insecure-port=8080
...
```

### Send Manual API Request

k config view --raw

echo LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJQVM1TEFhdDBWWXd3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TkRBek1qWXdNakExTlROYUZ3MHpOREF6TWpRd01qRXdOVE5hTUJVeApFekFSQmdOVkJBTVRDbXQxWW1WeWJtVjBaWE13Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLCkFvSUJBUURScVorekUvTnZXRVc0VVd6U1JIbitQenpacC9kNkUwN0JyOU9DaXc1OHQ4ZzU3dWk4ZytTdHVaTGwKSnBtZXlVbitwSkhvN3VVandCMGJ6a0hRNHJBY25OdTQ5dG5hVVIrVkVITjlkTU94enRDM3JzRGROUTJKT1kwZQpGWUZ6VDRpK3Y2UHBGTzlOaDBicWtzSlVaVU9LWXhPcXI4bnNmdU1HZlFVUXVIbm12Qk9LdldCMUVwbnBocDRJCjBMSXlkTXBaSGdjQ1NIU2xBd1RCL1QzdVpSaEVXZjdHS2FkK1BjcWFXRGZkdnJrSWtWNTg4elVTN25WZlNDVEEKbzRzcEJWODI2eHNNMGVNdTNqY3JYd1IvMlI0K0NvbjFZNzRzdjl6RXdEdzQ1QUpzaUNrKzVSOHU4V1pBV0RjTwo1a3p1UXhabUl1Szh6bUkzaHozWVVsVGtzc2Z0QWdNQkFBR2pXVEJYTUE0R0ExVWREd0VCL3dRRUF3SUNwREFQCkJnTlZIUk1CQWY4RUJUQURBUUgvTUIwR0ExVWREZ1FXQkJUOE5rLytDaDRrVmd4ODRqYlJtLzF5cVQrYmZUQVYKQmdOVkhSRUVEakFNZ2dwcmRXSmxjbTVsZEdWek1BMEdDU3FHU0liM0RRRUJDd1VBQTRJQkFRREczUk1SZzFYcQoxTVhFNlZJeXZ1RG1sN0M1bEtWbjNtalVzSmJrczZVaWtJenhXOEtTWFpDM3lYTnRrKzFYU0V2YVp0bjRmK2NtCnlkb1VxV1NHVVR5M3E3b3VNQXN0U2Z5WXBpVGtINFNHOWdEUDFYVUdkTkkvQnRpTEhQeTl4dnpHa0p2VDBIS0EKRGtlelpUN0lNejlwYVZ4MWxLZk9oTDJ2TWI2MFk5MkZPU3IwN0hadDZUOHVTM05HWjdqMGg2dEd0WWxwbW1VWApRYmQrUnVsVjZ4Z3lCdmNSMjNjcjhMcC9jS1lLZWdDbFR6d3FBdEl6eHYrUmY1MnNhNVNKZnNhMVlvUWZyck95CmxXdGdSMVFGQjdIVENxU0xONTg4QVNTNjFQcU1uZWZyc0RiendST0tKT0FlZ2h6TlhtV1htRFZvdXRlbmw3Y0YKdWV0WDhGaEROUDg3Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K | base64 -d > ca

echo LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURLVENDQWhHZ0F3SUJBZ0lJTTlPeUZ2TFdnNVl3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TkRBek1qWXdNakExTlROYUZ3MHlOVEF6TWpZd01qRXdOVGRhTUR3eApIekFkQmdOVkJBb1RGbXQxWW1WaFpHMDZZMngxYzNSbGNpMWhaRzFwYm5NeEdUQVhCZ05WQkFNVEVHdDFZbVZ5CmJtVjBaWE10WVdSdGFXNHdnZ0VpTUEwR0NTcUdTSWIzRFFFQkFRVUFBNElCRHdBd2dnRUtBb0lCQVFERmU4T3IKYmYrZitNRHFNakozczBFZGRuNVp4d2dLbTg0b0plWkpDNVBLVDhZYXlMWmNYckFFNXB4QUtUYXd3bFNiNTh1eQp2VTFtaVZkZ3AxSGx1WWI2TnU3YmFLalg0bCs2b2ZaZ3A1anFoYUhiVnhuay8xc2pQSHgvVTJTWkVGbWkyVjJOClFlYVE2OVNScWhJbHpSeHJnRWxTbU81dFFMd1d3SHFhbmpGcXZFYlErSEg3WFJ5ZHE1VjRNUk9HbXEyYlhBOVUKMUJHUEJia0VlTzhiMlpQVDJRRlA3VmpVb1RFNDBtUXFWRVZodFNESnMycnZZYm9zRGpYY21rV0M1Z2dOWTNOOQpaMWZQNU51UXdrbFo2MS8zZmJtQ0lsTGF2MGo3SmhaaXpxVGZqMWMxZXVRdHZoKzRkeDBsVVF2VUtNVE5LMjRJCnhRWWhyNWp5N1RSZTRIbUJBZ01CQUFHalZqQlVNQTRHQTFVZER3RUIvd1FFQXdJRm9EQVRCZ05WSFNVRUREQUsKQmdnckJnRUZCUWNEQWpBTUJnTlZIUk1CQWY4RUFqQUFNQjhHQTFVZEl3UVlNQmFBRlB3MlQvNEtIaVJXREh6aQpOdEdiL1hLcFA1dDlNQTBHQ1NxR1NJYjNEUUVCQ3dVQUE0SUJBUURPZkZVc3BOOVkwRnM2RlZBMmwwcHlxSnR1CjVOby93ZG9GV0FXdmxVckgwbWM3dVVZKzlxUmtQRDhuSDBlNEFKSVF6RkRYaG1HVUJDVm1RNGdrbDFYcmVzMEcKbkVyYjR6d1M5UkozS0Z0UElCTlIrVmgzK1c2NjdkdXJZMklCWEltL25XM0dZcytXemZKODRXL2VXMW1iUk00Qwp5Y0xwWnpPUHZieGlWM0oyOW5qSWk5UHBlaDUzVEtDUmxNOGdRSXgrMlBQZFZEVUVXSDJueUhEMEIxbHNCbk1DCnlNRno1ZHVmZW44dEtlWjUvME1qMkFacUM4UGQ2OTUxbjl5dEhmbWVJUkd1YnR5QkhkNDR0RU9XNC9KNVNRbkUKbjlGTHkvVGNpa1VRNGFUMU9QSVg3dTJLOFJ1Y1BYcmJDZjBvaktkR3ZtaEpJNE43UkhacmdxUUFFaCtNCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K | base64 -d > crt

echo LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcEFJQkFBS0NBUUVBeFh2RHEyMy9uL2pBNmpJeWQ3TkJIWForV2NjSUNwdk9LQ1htU1F1VHlrL0dHc2kyClhGNndCT2FjUUNrMnNNSlVtK2ZMc3IxTlpvbFhZS2RSNWJtRytqYnUyMmlvMStKZnVxSDJZS2VZNm9XaDIxY1oKNVA5Ykl6eDhmMU5rbVJCWm90bGRqVUhta092VWthb1NKYzBjYTRCSlVwanViVUM4RnNCNm1wNHhhcnhHMFBoeAorMTBjbmF1VmVERVRocHF0bTF3UFZOUVJqd1c1Qkhqdkc5bVQwOWtCVCsxWTFLRXhPTkprS2xSRlliVWd5Yk5xCjcyRzZMQTQxM0pwRmd1WUlEV056ZldkWHorVGJrTUpKV2V0ZjkzMjVnaUpTMnI5SSt5WVdZczZrMzQ5WE5YcmsKTGI0ZnVIY2RKVkVMMUNqRXpTdHVDTVVHSWErWTh1MDBYdUI1Z1FJREFRQUJBb0lCQVFDNnFCaHgyQzVkRGNtSgprcGlRK3lUNHJCOFF6RWFWZ0Y2REpBOWR5MHVOVllseGwzU0dLaGxGQ0pOM01YMDM1UFlEeGp1S1hkTGlyNzJlCjVZZExFdWk1WjJLc2oyZkhaWGdGOXovZ2E0amxZaGx5TUFtUm9LcUx5NGdBOE5tTXN4K0dCTjJmdmtJbmlFQUsKemkwSS9hMTNEbkkvVjcxRUZvT3hIWXpFeC9EOVovd1NMZkg4YXl6NlVHUXlvU3NoK0dQeFZjWjBuSGtLQmRrdApjZSt2U0hERm5HdGpkWlhXMlVPU2J1dDZnei9WWkYyaVM5MVp1SWdyM29TV0N0N3ZVRFlRb0UzYTNzNndyUXdUCjkrM01WaEhZUi83YUI1TzQvcGJIWlhSRHpCYTJsaHAwY3k1ZDExU3BVNGw0d3hNZmlaOUpOWWwwZ3dIbzR5MWoKZUErY2FxQUJBb0dCQVByTDJqWmVkTDdUelV2OUNuRnRQT0lveDQ1bUowTkxLZFc1ZTZGSzZOYWpsOFBhVXpzUQpRdUVuWmc2Y1gydUZOMXdxc0d3N1JKcWpjMUhhY1NCbW1OQlhoS3RETEtWZVVIRGZSMzlMMm9zSWZwaVN3STVpCmRkUFd5NzBWNGRBeDljVHE1NHJwcXlMeEFkbEQydXFEdW5JcnYwbzBoeE1CN0NWeFdOTlVPdGJoQW9HQkFNbVUKdTBzMVhtem9oeHUrWUlqQ3NpMkpKNGdoUi9GaUhZazMxV2pzRFFIRWMyeWJRWGlyYVFQUHdHTUJQOU9DdmtyMQpRN3RtS1hzSUNDaTV5anh4Z1psT2JQU2Q1RzU0VjVBVkZFb1dQOG1abjJKanhGNDAxS1pwc3lONFhnWnM4WXVpCm9Ua2dGRHZDZjNoRU12OTdHRE5vZ0trWUVTZzBxN2cvZDBXTWFCYWhBb0dBVjB2MTNNN3NIREJsV1hudTFLU04KZUx0eEd1UDc3clNQRDFITThzdThXRm9CVGE4RklaMzdhWnZwTGxUSDhna2d4L2drQ01ob2pOc3dIT0hJVnRyZgpma1c0YkZTcGliWldrYk5tazZ5M21ZV1BhMVJKcWtZamRXVmk3YUpjUTdmZ01IY0R1WnEwY3lrbzE1T0M4L1orClE1ZHVza211YXJOVW00UGt3MHFpWUlFQ2dZQjhSUmFack5NRGJPNHQ2bFYwdWlKQjlEWE10RWUzeFhiVDZ2bkQKYnhJdHJzQkJpZ3o0cVNOYVdDOFFXZXJSSjk3TU14dUlZZGpjb2Z6MXJtUEFrM0VENDlkRGpqc081MTJEMDVybwplWUxsYzdGUVpKVGdSczE1c2R2ZjJBcVBCNFo1UU04SGVvRSt2Zzc3UTMvMUJCdk5SWFZieVJ4Nm5zM21EaW9uClZBR3ZRUUtCZ1FEaHQyVUZpTURVRWptZHNmSjVqZWxHVjFudndMNlVHbjhBZmQ1dHE0Nkt3cWtROWhFN2JKbDYKU3FEYkFzQVJZZjUydVdPVzNwcmJUS2R6elhDbSt1bWZzazlDTkpSbzRteVNuOUNtS1RjL1dZcW5DcmpsNmludwoyNHUwQTI1MzJjSVQrN1UvNDNJd1dKc1JDdStTZ1krOExFQi9QTEhndjBWQ2I5OTRrNHR2TGc9PQotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQo= | base64 -d > key

curl https://10.156.0.2:6443 --cacert ca

### Send API Request from Outside

k --kubeconfig conf get ns

openssl x509 -in apiserver.crt -text


---

### NodeRestriction AdmissionController

NodeRestriction:
- Admission Controller:
  - kube-apiserver --enable-admission-plugins=NodeRestriction
  - Limits the Node labels a kubelet can modify

- Ensure secure workload isolation via labels
  - no one can pretend to be ˜secure˜ node and schedule secure pods

NodeRestriction in Action:
- Verify the NodeRestriction work
