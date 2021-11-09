# spring-boot-kafka-consumer

# 1. Dependências no pom.xml

```
<dependency>
   <groupId>org.apache.kafka</groupId>
   <artifactId>kafka-streams</artifactId>
</dependency>
<dependency>
   <groupId>org.springframework.kafka</groupId>
   <artifactId>spring-kafka</artifactId>
</dependency>
```

# 2. application.properties
```
# Consumer properties
spring.kafka.consumer.bootstrap-servers=127.0.0.1:9092
spring.kafka.consumer.group-id=group_id
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
topic.name.consumer=topico.comando.teste

# Common Kafka Properties
auto.create.topics.enable=true
```

Aqui tenho as mesmas observações do producer: StringDeserialize para trabalhar com textos e auto.create.topics.enable=true.
A classe consumer ficou assim:

```
@Slf4j
@RequiredArgsConstructor
@Service
public class TopicListener {

    @Value("${topic.name.consumer")
    private String topicName;

    @KafkaListener(topics = "${topic.name.consumer}", groupId = "group_id")
    public void consume(ConsumerRecord<String, String> payload){
log.info("Tópico: {}", topicName);
        log.info("key: {}", payload.key());
        log.info("Headers: {}", payload.headers());
        log.info("Partion: {}", payload.partition());
        log.info("Order: {}", payload.value());

    }
}
```

A anotação “@KafkaListener” permite conexão com um tópico para o recebimento de mensagens.
Rodando os projetos, só chamar a controller de testes e dá pra ver o resultado nos logs:


