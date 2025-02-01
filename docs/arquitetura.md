# Arquitetura

Para criar uma arquitetura com o **AWS Lambda** em um projeto de página web, você pode adotar uma abordagem **serverless** onde o backend é completamente gerido pela AWS, sem a necessidade de manter servidores. Aqui está uma descrição detalhada de como seria essa arquitetura:

### Arquitetura Serverless para uma Página Web com AWS Lambda:

1. **Frontend (Página Web)**
   - Sua página web pode ser uma aplicação estática (HTML, CSS, JavaScript) ou um framework moderno como React, Angular ou Vue.js.
   - O frontend envia **requisições HTTP** para a API que será gerenciada pela **API Gateway**, que por sua vez invoca funções do **AWS Lambda** para realizar as tarefas necessárias.
   - As requisições podem ser feitas usando **AJAX**, **fetch()**, ou bibliotecas como **Axios** no JavaScript.

2. **API Gateway**
   - O **Amazon API Gateway** é utilizado para criar uma **API REST** que serve como um ponto de entrada para as requisições HTTP feitas pelo frontend.
   - A API Gateway expõe **endpoints** que podem ser acessados via URLs HTTP, como `GET /usuario`, `POST /cadastrar`, etc.
   - A API Gateway é configurada para **invocar funções Lambda** sempre que uma requisição HTTP é recebida.

3. **AWS Lambda**
   - **Funções Lambda** são responsáveis por executar a lógica de backend. Elas são invocadas pela **API Gateway** em resposta a eventos HTTP.
   - Cada função Lambda pode ser programada para realizar tarefas específicas, como:
     - Processamento de dados recebidos do frontend (ex: cálculos, validações, transformação de dados).
     - Interação com outros serviços AWS (ex: armazenar dados no **DynamoDB**, enviar emails com **Amazon SES**, processar arquivos no **S3**, etc.).
   - **Escalabilidade automática**: O Lambda pode lidar com um grande volume de requisições simultâneas, escalando automaticamente sem necessidade de intervenção manual.

4. **Banco de Dados - Amazon DynamoDB** (ou outro banco)
   - O **Amazon DynamoDB** é uma excelente opção para aplicações serverless, pois é um banco de dados NoSQL totalmente gerenciado e escalável.
   - Sua função Lambda pode interagir com o DynamoDB para armazenar e recuperar dados. Por exemplo, se você estiver criando um sistema de cadastro de usuários, o Lambda pode escrever as informações no DynamoDB.
   - Em alternativas mais complexas, você também pode usar o **RDS** (para bancos relacionais) ou até o **Aurora Serverless**, dependendo da sua necessidade.

5. **Amazon S3 (para arquivos estáticos ou uploads)**
   - Caso sua aplicação precise lidar com upload de arquivos (ex: imagens, documentos), você pode usar o **Amazon S3**.
   - O frontend pode enviar os arquivos diretamente para o S3 via uma URL gerada pela API Gateway e Lambda, ou o Lambda pode manipular os uploads de maneira automatizada.

6. **Autenticação e Autorização (opcional)**
   - Se sua aplicação precisar de autenticação de usuários, você pode integrar o **Amazon Cognito** para gerenciamento de usuários e autenticação.
   - O Cognito pode ser configurado com o **API Gateway** para garantir que apenas usuários autenticados acessem certos endpoints.

7. **CloudWatch (para monitoramento e logs)**
   - O **AWS CloudWatch** pode ser usado para monitorar a execução das funções Lambda, coletar logs, e definir alarmes para notificá-lo de erros ou picos de tráfego.
   - Você pode também configurar métricas e logs personalizados para depuração e análise do comportamento da aplicação.

---

### Fluxo de Dados:

1. **Requisição do Frontend:**
   - O usuário interage com a página web (ex: submete um formulário ou faz login).
   - O frontend envia uma requisição HTTP (usando `fetch()`, `Axios`, etc.) para o **API Gateway** (ex: POST `/cadastrar`).

2. **API Gateway:**
   - O **API Gateway** recebe a requisição e a roteia para a função Lambda apropriada.
   - O Gateway também pode fazer a validação dos dados recebidos, como autenticação via **Cognito**, validações de CORS, etc.

3. **Função Lambda:**
   - A função Lambda é invocada e executa a lógica necessária (ex: validação dos dados, interação com o banco de dados, processamento de imagens, etc.).
   - A função Lambda pode interagir com o **DynamoDB** (ou outro banco) para armazenar ou recuperar dados. Ela também pode interagir com outros serviços, como **S3** para uploads ou **SNS** para envio de notificações.

4. **Resposta para o Frontend:**
   - A função Lambda retorna a resposta ao **API Gateway**, que por sua vez envia essa resposta de volta para o frontend.
   - O frontend processa a resposta e atualiza a interface com o resultado (ex: exibe uma mensagem de sucesso ou erro).

5. **Monitoramento (opcional):**
   - O CloudWatch pode monitorar o desempenho da função Lambda, como o tempo de execução e possíveis falhas.
   - Logs e métricas também podem ser enviados ao CloudWatch para análise detalhada.

---

### Exemplo de Arquitetura Visual:

1. **Frontend (Página Web)** → envia requisições HTTP via **AJAX/Fetch**.
2. **API Gateway** → define os **endpoints** RESTful e invoca funções **Lambda**.
3. **Função Lambda** → executa a lógica e interage com o **DynamoDB**, **S3**, etc.
4. **Banco de Dados (DynamoDB)** → armazena e consulta os dados necessários.
5. **S3** (se necessário) → armazena arquivos como imagens ou documentos enviados pelo usuário.
6. **CloudWatch** → monitora logs e métricas de desempenho.

---

### Benefícios dessa Arquitetura:

- **Escalabilidade automática**: O Lambda escala automaticamente com base no número de requisições, sem precisar de servidores.
- **Baixo custo**: Você paga apenas pelo tempo de execução das funções Lambda, sem custos fixos com servidores.
- **Desempenho e agilidade**: Com o Lambda, a resposta é muito rápida, pois não há servidores dedicados para gerenciar.
- **Foco no código**: Como o backend é gerido pela AWS, você pode se concentrar apenas na lógica de negócios, sem se preocupar com a infraestrutura.

Essa arquitetura é ideal para aplicações que requerem alta escalabilidade, com mínima manutenção de infraestrutura e baixo custo, aproveitando os recursos da AWS de forma otimizada.

[Próximo passo... Criando uma função](create.md)