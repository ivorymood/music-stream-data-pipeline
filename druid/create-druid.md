kubectl create namespace druid
helm repo add wiremind https://wiremind.github.io/wiremind-helm-charts

helm install -n druid druid-chart wiremind/druid --version 1.0.0

