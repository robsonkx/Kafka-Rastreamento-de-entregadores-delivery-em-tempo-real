#### **Objetivo:**

Você deve implementar uma pipeline de dados para **rastreamento de entregadores de um aplicativo de delivery em tempo real**. O sistema precisa ser eficiente: o histórico completo de movimentação é irrelevante para o despacho imediato; o que importa para a operação é saber onde cada entregador está exatamente agora.

Sua tarefa é

1.configurar um cluster Kafka e uma pipeline de integração que garanta que o estado mais recente de cada entregador esteja disponível de forma rápida e persistente em um banco de dados em memória (Redis).


#### **Requisitos Técnicos:**

##### **1. Infraestrutura Kafka**

- Suba um cluster Kafka com 3 nós (brokers) para garantir tolerância a falhas.
- O sistema deve ser configurado de forma que o Kafka não armazene o histórico infinito de posições, mas sim a última localização conhecida por entregador. Cabe a você configurar o tópico adequadamente para este comportamento de otimização de armazenamento.

##### **2. Ingestão de Dados (Produtor)**

- Utilize o código Python fornecido que simula a geração de eventos de localização.
- Você deverá desenvolver o produtor que envia essas mensagens para o Kafka, garantindo que a estrutura da mensagem permita a atualização correta do estado no destino.
- Esquema da Mensagem\*\*:\*\* `driver_id`, `latitude`, `longitude`, `timestamp`, `status`.

##### **3. Integração com Redis (Kafka Connect)**

- Configure o Kafka Connect (Sink Connector) para realizar o transporte automático dos dados do Kafka para o Redis.
- O Redis deve atuar como um espelho do estado atual. Para cada `driver_id`, deve existir apenas um registro refletindo sua posição e status mais recentes.

##### **4. Monitoramento (Prometheus & Grafana)**

- Implemente o monitoramento do ambiente usando Prometheus.
- Forneça um Dashboard no Grafana com as métricas mais relevantes.

#### **O que você deve entregar:**

1. **Topologia do Ambiente:**  Arquivo `docker-compose.yml` ou documentação dos comandos de instalação/arquivos de configuração (Cluster Kafka, Connect, Redis e Monitoramento).
2. **Configuração de Tópicos:**  Documente o comando de criação dos tópicos.
3. **Configuração do Conector:**  O arquivo JSON (ou comando `curl`) utilizado para instanciar o conector Redis Sink.
4. **Evidência de Monitoramento:**  Screenshots do dashboard do Grafana.
5. **Validação de Estado:**  Demonstração (via `redis-cli`) de que o banco contém informações atualizadas e sem duplicidade por entregador.
6. **Vídeo Explicativo:**  Explique a arquitetura montada e as decisões de configuração de tópico.

   1. Explique como você garantiu a unicidade dos dados no Redis.
   2. Demonstração de Resiliência: Durante a gravação, pare uma das instâncias (nós) do Kafka e exiba em tempo real o impacto disso no seu dashboard de monitoramento.


---
