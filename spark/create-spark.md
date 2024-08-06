```sh
## 스파크 클러스터 생성 명령어
kubectl create namespace spark
helm -n spark install spark-chart oci://registry-1.docker.io/bitnamicharts/spark

## 출력결과
# Pulled: registry-1.docker.io/bitnamicharts/spark:9.2.8
# Digest: sha256:37347bfba8a68bc4f50a4d6ca73fc0fd632ef1be6e58b4b5b224a222907d5f97
# NAME: spark-chart
# LAST DEPLOYED: Wed Aug  7 01:42:10 2024
# NAMESPACE: spark
# STATUS: deployed
# REVISION: 1
# TEST SUITE: None
# NOTES:
# CHART NAME: spark
# CHART VERSION: 9.2.8
# APP VERSION: 3.5.1
# 
# ** Please be patient while the chart is being deployed **
# 
# 1. Get the Spark master WebUI URL by running these commands:
#   kubectl port-forward --namespace spark svc/spark-chart-master-svc 80:80
#   echo "Visit http://127.0.0.1:80 to use your application"
# 
# 2. Submit an application to the cluster:
#   To submit an application to the cluster the spark-submit script must be used. That script can be
#   obtained at https://github.com/apache/spark/tree/master/bin. Also you can use kubectl run.
# 
#   export EXAMPLE_JAR=$(kubectl exec -ti --namespace spark spark-chart-worker-0 -- find examples/jars/ -name 'spark-example*\.jar' | tr -d '\r')
# 
#   kubectl exec -ti --namespace spark spark-chart-worker-0 -- spark-submit --master spark://spark-chart-master-svc:7077 \
#     --class org.apache.spark.examples.SparkPi \
#     $EXAMPLE_JAR 5
# 
# ** IMPORTANT: When submit an application from outside the cluster service type should be set to the NodePort or LoadBalancer. **
# ** IMPORTANT: When submit an application the --master parameter should be set to the service IP, if not, the application will not resolve the master. **
# 
# WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the 
# following values according to your workload needs:
#   - master.resources
#   - worker.resources
# +info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/

```

```sh
## 클러스터 생성결과 확인
kubectl -n spark get pod

## 출력결과
# NAME                   READY   STATUS    RESTARTS   AGE
# spark-chart-master-0   1/1     Running   0          3m26s
# spark-chart-worker-0   1/1     Running   0          3m26s
# spark-chart-worker-1   1/1     Running   0          116s

## 서비스 생성결과 확인
kubectl -n spark get service

## 출력결과
# NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)           AGE
# spark-chart-headless     ClusterIP   None           <none>        <none>            6m42s
# spark-chart-master-svc   ClusterIP   10.96.218.66   <none>        7077/TCP,80/TCP   6m42s


```


```sh
# 웹 포트포워딩
kubectl -n spark port-forward service/spark-chart-master-svc 8082:80
```
