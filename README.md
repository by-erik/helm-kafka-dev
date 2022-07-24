# Kafka Dev

## Create a topic

```bash
kubectl run -it --rm --image=confluentinc/cp-enterprise-kafka:6.1.0 createtopic --restart=Never -- \
  kafka-topics \ 
    --topic test \
    --create \
    --bootstrap-server kafka-dev-cp-kafka:9092 \
    --partitions 1 \
    --replication-factor 1
```

## Consume a topic

```bash
kubectl run -it --rm --image=confluentinc/cp-enterprise-kafka:6.1.0 consoleconsumer --restart=Never -- \
  kafka-console-consumer \
    --topic test \
    --bootstrap-server kafka-dev-cp-kafka:9092 \
    --from-beginning \
    --property print.key=true \
    --property key.separator=":"
```

## Produce on topic

```bash
kubectl run -it --rm --image=confluentinc/cp-enterprise-kafka:6.1.0 consoleproducer --restart=Never -- \
  kafka-console-producer \
    --topic test \
    --bootstrap-server kafka-dev-cp-kafka:9092 \
    --property parse.key=true \
    --property key.separator=":"
```

### Multiline non blocking
```bash
kubectl run -it --rm --image=confluentinc/cp-enterprise-kafka:6.1.0 consoleproducer --restart=Never -- \
  bash -c '\
    cat <<EOF | \
    tr "\n" " " | \
    kafka-console-producer \
        --topic test \
        --bootstrap-server kafka-dev-cp-kafka:9092 \
        --property parse.key=true \
        --property key.separator=":"
multiline:{
  "message": "Hallo"
}
EOF'
```