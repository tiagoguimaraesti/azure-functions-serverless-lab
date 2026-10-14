# ☁️ Azure Functions Serverless & Application Insights

## 🎯 Objetivo do Projeto
Este laboratório prático documenta a criação, o *deployment*, a segurança e a monitorização de uma API Serverless no Microsoft Azure. Este projeto foi desenvolvido para consolidar os fundamentos de infraestrutura em nuvem, alinhando a componente académica de Redes de Computadores com as exigências do mercado europeu de Cloud Computing. O portefólio prático integra a preparação contínua para as certificações **AZ-900** e **AZ-104**.

## 🛠️ Tecnologias e Serviços Utilizados
* **Microsoft Azure:** Azure Functions (Plano Consumption), Application Insights, Log Analytics Workspace.
* **Desenvolvimento e Stack:** Node.js (Modelo V4), HTTP Triggers.
* **Ferramentas:** Azure CLI, Azure Cloud Shell (Bash), Kusto Query Language (KQL).

---

## 🚀 Etapas de Implementação e Validação

### 1. Provisionamento e Deployment (Azure CLI)
A infraestrutura base foi provisionada na região `Central Mexico`. O ambiente de desenvolvimento foi configurado no Cloud Shell, onde um projeto Node.js foi inicializado e a função `GetStatus` foi criada com um gatilho HTTP (*HTTP Trigger*). O código foi publicado com sucesso utilizando o comando `func azure functionapp publish`.

<img width="1909" height="704" alt="Captura de tela 2026-10-13 221250" src="https://github.com/user-attachments/assets/c46bc742-f07d-4190-892a-8567eca0d872" />


### 2. Validação da Implantação no Portal
Após o envio do código via linha de comandos (CLI), a função foi validada através do *Azure Resource Manager* no portal do Azure, confirmando a correta associação do gatilho HTTP à Function App.

### 3. Observabilidade e Telemetria Nativa
O monitoramento da aplicação Serverless foi configurado com o **Application Insights**, gravando os dados num **Log Analytics Workspace**. A telemetria capturou automaticamente as métricas de desempenho (APM) e os tempos de resposta das invocações HTTP sem necessidade de alterar o código da aplicação.

<img width="1919" height="804" alt="Captura de tela 2026-10-13 215008" src="https://github.com/user-attachments/assets/fe8dc369-0dcd-4119-a099-6175e9cbcea4" />


### 4. Segurança e Restrição de Acesso (API Protection)
Para evitar custos desnecessários e proteger o acesso público ao endpoint, o nível de autorização foi modificado de `anonymous` para `function`.
* **Cenário Anónimo:** Qualquer utilizador podia executar a função através do navegador.
* **Cenário Seguro (Function Key):** O acesso público foi bloqueado (retornando o erro de bloqueio de rede `HTTP 401 Unauthorized`). O acesso autorizado foi reestabelecido apenas com a injeção da *Function Key* como parâmetro `?code=` na Query String da requisição.

### 5. Análise de Logs Avançada (KQL)
Utilização de consultas em Kusto Query Language (KQL) para interrogar o Log Analytics. A query `requests | order by timestamp desc` foi utilizada para listar detalhadamente o tráfego HTTP, o código de sucesso (`200 OK`) e o tempo em milissegundos que a infraestrutura demorou a responder, comprovando a eficácia e a rapidez da arquitetura Serverless.

<img width="1909" height="918" alt="Captura de tela 2026-10-13 224804" src="https://github.com/user-attachments/assets/f8132c1c-f246-4293-8a03-168cb209530f" />


### 6. Governança e Otimização de Custos (Clean-up)
Como parte das boas práticas de administração de nuvem pública (FinOps), ao concluir a validação arquitetónica, todos os Grupos de Recursos (`rg-gp-functions-endpoint` e os respetivos grupos de monitorização) foram destruídos de forma permanente. Este processo garante que não há recursos "órfãos" a gerar custos inesperados na subscrição.
