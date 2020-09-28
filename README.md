# install kafka using strimzi

//add helm chart repo for Strimzi
```
helm repo add strimzi https://strimzi.io/charts/
```
//install it! 
```
helm install strimzi-kafka strimzi/strimzi-kafka-operator
```
// install kafka
```
kubectl apply -f kafka-cluster.yml
```
# Install schemaregistry 
```
helm repo add incubator https://kubernetes-charts-incubator.storage.googleapis.com
helm repo update
helm install my-sreg -f local-helm-value.yml incubator/schema-registry 
```
# Run producer and consumer as a deployment 
```
kubectl run sgtest-container -it --image=proy/sg-kafka:v1
```
# test python-avro-producer, go inside the container name sgtest-container 
```
cd /code/python-avro-producer


python send_record.py --topic test --bootstrap-servers dhs-event-manager-kafka-bootstrap:9092 --schema-file create-user-request.avsc --record-value '{"email": "email@email.com", "firstName": "Pradeep", "lastName": "Kumar"}'


python send_record.py --topic test --bootstrap-servers dhs-event-manager-kafka-bootstrap:9092 --schema-registry http://dhs-sg-schema-registry:8081 --schema-file create-user-request.avsc --record-value '{"email": "email@email.com", "firstName": "Pradeep", "lastName": "Kumar"}'
``
#python consume_record.py --topic test --bootstrap-servers my-kafka-cluster-kafka-bootstrap:9092 --schema-file create-user-request.avsc

# schemaregistry and kafka broker test from kafka broker container 

```
```
curl -X GET http://my-sreg-schema-registry:8081/subjects

curl -X GET http://my-sreg-schema-registry:8081/subjects/test-value/versions/1


./kafka-topics.sh --list --bootstrap-server localhost:9092
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning

```