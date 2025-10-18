# desafio2-codegirls
Repositório para o desafio 2 do curso Santander Code Girls

### Reflexões sobre o Aprendizado Prático com AWS Step Functions

A experiência de desenvolver um fluxo de trabalho na AWS, utilizando o **Step Functions** e o **Workflow Studio**, proporcionou um aprendizado prático e aprofundado em orquestração de serviços *serverless* e lógica de aplicações distribuídas.

O projeto, encapsulado na Máquina de Estado denominada `StateMachineForChallenge`, demonstra a capacidade de encadear operações complexas de forma declarativa e visual.

**Aprendizados Chave e Implementações Realizadas:**

1.  **Orquestração de Múltiplos Serviços AWS:** O fluxo não se limitou a uma única função, mas integrou e coordenou ativamente três domínios de serviço essenciais:
    * **Step Functions (Task `StartExecution`):** A inclusão desta tarefa no início do fluxo demonstrou o domínio em arquiteturas aninhadas, onde um fluxo de trabalho principal pode disparar e monitorar sub-fluxos, ideal para modularizar grandes processos ou lidar com limites de execução.
    * **Amazon EC2 (Task `CreateInstanceExportTask`):** A utilização de uma tarefa EC2 aponta para a manipulação de infraestrutura, como iniciar um processo de exportação de imagem de máquina (AMI), o que exige a correta configuração de permissões e a gestão do ciclo de vida de recursos de computação.
    * **Amazon S3 (Task `CreateBucket`):** A etapa final de criar um bucket S3 ilustra uma ação fundamental de armazenamento. A configuração dessa tarefa na aba "Arguments & Output" exigiu o entendimento de como o Step Functions utiliza a integração otimizada com o AWS SDK, permitindo passar parâmetros dinamicamente do resultado das etapas anteriores.

2.  **Modelagem Visual e Linguagem de Estados (ASL):**
    * A utilização do **Workflow Studio (Visual Designer)** permitiu a construção rápida do esqueleto do fluxo (`Start -> Step Functions -> EC2 -> S3 -> End`), reforçando a agilidade no desenvolvimento *serverless*.
    * Simultaneamente, a capacidade de alternar para a aba **Code** (onde reside o JSON da **Amazon States Language - ASL**) reforçou a compreensão da sintaxe subjacente. Isso é crucial para ajustes finos em manipulação de dados (`InputPath`, `ResultPath`, `OutputPath`) e implementação de lógica avançada (como `Catch` e `Retry` para tratamento de erros e resiliência).

3.  **Configuração de Permissões (IAM Role):**
    * Embora a imagem mostre a arquitetura, a implementação bem-sucedida de um projeto como este exigiu a criação ou ajuste de uma **IAM Role** (Função de Execução) para o Step Functions. Este foi um ponto de atenção crítica para garantir que a Máquina de Estado tivesse as políticas mínimas de privilégio para executar as ações de `sfn:StartExecution`, `ec2:CreateInstanceExportTask` e `s3:CreateBucket`.

4.  **Processamento de Dados e Fluxo de Controle:**
    * A transição de dados entre os estados (`Output` de um estado servindo como `Input` para o próximo) foi gerenciada. Por exemplo, o ID da `CreateInstanceExportTask` provavelmente foi extraído e formatado para a etapa subsequente, demonstrando o controle preciso sobre o *payload* da execução.

Em resumo, este projeto prático não apenas resultou em um fluxo de trabalho funcional, mas também solidificou a compreensão de conceitos arquiteturais avançados, como orquestração *serverless*, tratamento de erros, segurança com IAM, e a integração profunda entre os principais serviços da AWS.
