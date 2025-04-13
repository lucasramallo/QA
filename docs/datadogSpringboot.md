# 📊 Integração do Datadog com Spring Boot usando Micrometer

Esta documentação orienta como integrar o **Datadog** com uma aplicação **Spring Boot**, utilizando o **Micrometer** para coleta de métricas e o **Spring Boot Actuator** para exposição dessas métricas.

## 🧰 Dependências

Adicione as dependências a seguir ao seu projeto para habilitar o envio de métricas para o Datadog.

### 💡 Maven

```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-registry-datadog</artifactId>
  <scope>runtime</scope>
</dependency>

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

<dependency>
  <groupId>com.datadoghq</groupId>
  <artifactId>dd-trace-api</artifactId>
  <version>LATEST</version>
</dependency>
```
`micrometer-registry-datadog:`Essa dependência é o conector entre o Micrometer e o Datadog. Ela permite que as métricas coletadas pela aplicação (como número de requisições, tempo de resposta, uso de memória etc.) sejam enviadas automaticamente para o Datadog

`spring-boot-starter-actuator:` Esse starter adiciona endpoints de monitoramento e gerenciamento na sua aplicação Spring Boot.
⚙️ Exemplos de endpoints:

- `/actuator/health – status da aplicação`

- `/actuator/metrics – métricas gerais (latência, memória, threads)`

- `/actuator/loggers – controle de níveis de log`

`dd-trace-api:` Essa é a API de tracing da Datadog, usada para fazer instrumentação manual do código.

💡 Você pode usar a anotação `@Trace` para marcar métodos específicos e gerar spans personalizados que aparecem no painel de tracing do Datadog.

### application.properties

```properties
spring.application.name=pocdatadog

management.datadog.metrics.export.enabled=true
management.datadog.metrics.export.api-key={sua-api-key}
management.datadog.metrics.export.application-key={sua-app-key}
management.datadog.metrics.export.uri=https://us5.datadoghq.com // Alere conforme a região escolhida ao criar a conta no datadog
management.datadog.metrics.export.step=30s
management.datadog.metrics.export.host-tag=instance

management.tracing.sampling.probability=1.0
dd.trace.global.tags=env:dev,version:1.0
dd.trace.health.metrics.enabled=true

logging.file.name=application.log
management.metrics.tags.service=meu-app
management.endpoints.web.exposure.include=health,metrics
```
# Rodando o Datadog Agent com Docker

Para coletar métricas, logs e traços da sua aplicação, você precisa do **Datadog Agent** rodando no seu ambiente. O Datadog Agent é responsável por coletar e enviar os dados para o Datadog, permitindo o monitoramento eficaz de suas aplicações.

## Requisitos

- **Docker** instalado e configurado corretamente.
- Uma **API Key** do Datadog (`DD_API_KEY`).
- **Site** correto do Datadog conforme sua região (ex: `us5.datadoghq.com`).

### Monitorando Endpoints
 Agora você pode monitorar endpoints especificos

```java
package com.lucasramallo.pocdatadog;

import datadog.trace.api.Trace;
import io.micrometer.core.annotation.Timed;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    @Timed("api.hello")
    @Trace(operationName = "hello.request")
    @GetMapping("/hello")
    public String hello() {
        return "Hello, Datadog!";
    }
}X

```

No Datadog acesse `Metrics > Explorer`

![Captura de tela de 2025-04-13 12-55-45](https://github.com/user-attachments/assets/9bf71940-9253-41cd-a1dd-2ae06c0b9254)


#### Filtre as metricas conforme o necessário

![Captura de tela de 2025-04-13 12-59-17](https://github.com/user-attachments/assets/303b944a-3eec-435a-aa88-f26bef7baf76)

**Métrica:** `http.server.requests.count` rastreia **o número total de requisições HTTP** recebidas por um serviço. 
- **Filtros aplicados:**  
  - `service:meu-app` (apenas do serviço "meu-app")  
  - `uri:/hello` (apenas requisições para o endpoint `/hello`)  

**Objetivo:**  
Monitorar o **volume total de requisições HTTP** para o endpoint `/hello` no serviço especificado.  

#### Assim as metricas ja serão apresentadas
![Captura de tela de 2025-04-13 13-02-50](https://github.com/user-attachments/assets/938b73f9-6582-42f5-9e62-5e2f9d9ae147)
