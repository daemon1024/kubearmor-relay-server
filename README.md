# KubeArmor Relay Server

KubeArmor's relay server collects all messages, alerts, and system logs generated by KubeArmor in each node, and then it allows other logging systems to simply collect those through the service ('kubearmor.kube-system.svc') of the relay server.

By default, the relay server is deployed with KubeArmor.

![Kubearmor Relay Server HLD](docs/relay-server.png)

## Streaming Kubearmor events to external SIEM tools

KubeArmor emits following types of events:
1. **Alert**: When policy is violated
2. **Log**: When a pod executes a syscall or any other action (such as file access, process creation, network socket create/connect/accept etc)
3. **Message**: Internal Kubearmor daemon messages

There are two approaches that one can take to stream the kubearmor events.
1. Using kubearmor-relay stdout: This is the easiest way i.e. if the SIEM tool connects to the k8s pod logging interface then all the kubearmor events (across all nodes) are available at the kubearmor-relay stdout. [Fluentd](https://docs.fluentd.org/v/0.12/articles/kubernetes-fluentd)/[SentinelOne](https://www.sentinelone.com/blog/get-started-quickly-with-kubernetes-logging/) does support this mode wherein .
2. Creating an adapter for the SIEM tool. Kubearmor-relay events could be accessed using its GRPC server ([ref code](https://github.com/kubearmor/kubearmor-client/tree/main/log)) and then the events could be streamed to the SIEM tool (splunk/elk/sentinelone ...).

<img src="docs/kubearmor-event-stream-arch.png" width="512">

> SentinelOne is used as an example in this figure
