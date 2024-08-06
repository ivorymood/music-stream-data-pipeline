```sh
## kafka 클러스터 생성 명령어
kubectl create namespace kafka

#helm pull oci://registry-1.docker.io/bitnamicharts/kafka --untar
helm install -n kafka kafka-chart oci://registry-1.docker.io/bitnamicharts/kafka

## 출력결과
# Pulled: registry-1.docker.io/bitnamicharts/kafka:30.0.0
# Digest: sha256:be8aa16c3b19b5cb97adecd8471733998448be988db1a473f92f1319c0efcffd
# NAME: kafka-chart
# LAST DEPLOYED: Wed Aug  7 01:58:53 2024
# NAMESPACE: kafka
# STATUS: deployed
# REVISION: 1
# TEST SUITE: None
# NOTES:
# CHART NAME: kafka
# CHART VERSION: 30.0.0
# APP VERSION: 3.8.0
# 
# ** Please be patient while the chart is being deployed **
# 
# Kafka can be accessed by consumers via port 9092 on the following DNS name from within your cluster:
# 
#     kafka-chart.kafka.svc.cluster.local
# 
# Each Kafka broker can be accessed by producers via port 9092 on the following DNS name(s) from within your cluster:
# 
#     kafka-chart-controller-0.kafka-chart-controller-headless.kafka.svc.cluster.local:9092
#     kafka-chart-controller-1.kafka-chart-controller-headless.kafka.svc.cluster.local:9092
#     kafka-chart-controller-2.kafka-chart-controller-headless.kafka.svc.cluster.local:9092
# 
# The CLIENT listener for Kafka client connections from within your cluster have been configured with the following security settings:
#     - SASL authentication
# 
# To connect a client to your Kafka, you need to create the 'client.properties' configuration files with the content below:
# 
# security.protocol=SASL_PLAINTEXT
# sasl.mechanism=SCRAM-SHA-256
# sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required \
#     username="user1" \
#     password="$(kubectl get secret kafka-chart-user-passwords --namespace kafka -o jsonpath='{.data.client-passwords}' | base64 -d | cut -d , -f 1)";
# 
# To create a pod that you can use as a Kafka client run the following commands:
# 
#     kubectl run kafka-chart-client --restart='Never' --image docker.io/bitnami/kafka:3.8.0-debian-12-r0 --namespace kafka --command -- sleep infinity
#     kubectl cp --namespace kafka /path/to/client.properties kafka-chart-client:/tmp/client.properties
#     kubectl exec --tty -i kafka-chart-client --namespace kafka -- bash
# 
#     PRODUCER:
#         kafka-console-producer.sh \
#             --producer.config /tmp/client.properties \
#             --broker-list kafka-chart-controller-0.kafka-chart-controller-headless.kafka.svc.cluster.local:9092,kafka-chart-controller-1.kafka-chart-controller-headless.kafka.svc.cluster.local:9092,kafka-chart-controller-2.kafka-chart-controller-headless.kafka.svc.cluster.local:9092 \
#             --topic test
# 
#     CONSUMER:
#         kafka-console-consumer.sh \
#             --consumer.config /tmp/client.properties \
#             --bootstrap-server kafka-chart.kafka.svc.cluster.local:9092 \
#             --topic test \
#             --from-beginning
# 
# WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the 
# following values according to your workload needs:
#   - controller.resources
# +info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
```


```sh
## client.properties 파일생성
## kafka client 생성
kubectl run kafka-chart-client --restart='Never' --image docker.io/bitnami/kafka:3.8.0-debian-12-r0 --namespace kafka --command -- sleep infinity
## 출력결과
# pod/kafka-chart-client created

kubectl cp --namespace kafka ./kafka/client.properties kafka-chart-client:/tmp/client.properties
```

```sh
## pv 볼륨 생성

## pvc 확인
kubectl --namespace kafka get pvc

# NAME                            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
# data-kafka-chart-controller-0   Bound    pvc-e4c5ac56-953f-4fc9-a20c-cf1a0bfb4fb5   8Gi        RWO            standard       <unset>                 12m
# data-kafka-chart-controller-1   Bound    pvc-4d8eb0db-6326-4278-b6b9-8d89d527eb52   8Gi        RWO            standard       <unset>                 12m
# data-kafka-chart-controller-2   Bound    pvc-e45e48bc-7948-494a-af18-6476f28568a8   8Gi        RWO            standard       <unset>                 12m

```

# mkdir permission denied 시
helm install [NAME] --set volumePermissions.enabled=true oci://registry-1.docker.io/bitnamicharts/kafka
