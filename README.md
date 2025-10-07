# dio-aws-arquitetura-agrotech

## Desafio DIO: Arquitetura de Nuvem para Agrotech na AWS

> Solução de Agrotech na AWS para análise de imagens de drones, utilizando S3, Lambda, EC2 e EBS. Projeto desenvolvido para o bootcamp da DIO na Formação AWS Cloud Foundations.

#Sobre o Projeto

Este projeto propõe uma arquitetura de nuvem escalável e automatizada para uma empresa de agronegócio ou consultoria agrícola que utiliza drones para monitorar a saúde de lavouras. A solução visa resolver o desafio de processar um grande volume de imagens de alta resolução de forma eficiente, gerando insights valiosos para a agricultura de precisão.

O pipeline de dados foi desenhado para ser resiliente e custo-efetivo, orquestrando diferentes serviços da AWS para cada etapa do processo, desde o upload dos dados brutos até a disponibilização dos resultados para o analista.

Este repositório documenta a arquitetura idealizada como parte do desafio de projeto da **Formação AWS Cloud Foundations** da [Digital Innovation One (DIO)](https://www.dio.me/).

## Arquitetura da Solução

O fluxo de dados foi desenhado da seguinte maneira:

1.  **Coleta e Upload:** Um operador de drone, após sobrevoar uma área da fazenda, faz o upload de um pacote de imagens (`.zip`) para um bucket no **Amazon S3**, que serve como a área de recebimento de dados brutos.

2.  **Gatilho e Pré-processamento:** A chegada do novo arquivo no S3 aciona automaticamente uma função **AWS Lambda**. Esta função é responsável por tarefas rápidas: descompactar o arquivo, validar os metadados das imagens e movê-las para um segundo bucket S3, destinado à etapa de análise.

3.  **Análise e Processamento:** Uma instância **Amazon EC2**, otimizada para computação pesada, é notificada sobre a chegada das novas imagens. Ela executa o trabalho de análise intensiva, como a aplicação de algoritmos de visão computacional para calcular o índice de saúde da vegetação (NDVI).

4.  **Armazenamento e Visualização:** A instância EC2 utiliza um volume **Amazon EBS** para armazenar o software de análise e arquivos temporários. Após o processamento, os resultados são salvos em um banco de dados, que por sua vez alimenta um dashboard para que o consultor agrícola possa visualizar os "mapas de calor" da lavoura e tomar decisões estratégicas.

## 🖼️ Diagrama da Arquitetura

A imagem abaixo ilustra o fluxo de dados e a interação entre os serviços da AWS:

\<details\>
\<summary\>Ver Código Mermaid do Diagrama\</summary\>

```mermaid
graph TD
    subgraph "Campo / Fazenda"
        A[<fa:fa-helicopter> Drone]
    end

    subgraph "Nuvem AWS"
        B[<fa:fa-archive> Bucket S3<br>Dados Brutos]
        C[<fa:fa-bolt> AWS Lambda<br>Pré-processamento]
        D[<fa:fa-archive> Bucket S3<br>Análise Pendente]
        E[<fa:fa-server> Instância EC2<br>Análise de Imagens (NDVI)]
        F[<fa:fa-hdd> Volume EBS<br>Software/Cache]
        G[<fa:fa-database> Banco de Dados<br>Resultados (RDS)]
        H[<fa:fa-chart-line> Dashboard<br>Consultor Agrícola]
    end

    A -- "1. Upload de imagens.zip" --> B
    B -- "2. Trigger de Evento" --> C
    C -- "Valida e move imagens" --> D
    D -- "3. Inicia processamento" --> E
    E <--> F
    E -- "4. Grava resultados" --> G
    G -- "5. Alimenta visualizações" --> H

    style A fill:#cde4ce,stroke:#333
    style H fill:#cde4ce,stroke:#333
```

\</details\>

## 🛠️ Tecnologias Utilizadas

  * **[Amazon S3 (Simple Storage Service)](https://aws.amazon.com/pt/s3/)**: Utilizado como um repositório de objetos escalável para armazenar as imagens brutas enviadas pelos drones e os arquivos já processados.
  * **[AWS Lambda](https://aws.amazon.com/pt/lambda/)**: Serviço de computação *serverless* acionado por eventos. Usado para o pré-processamento rápido (descompactação e validação) das imagens sem a necessidade de gerenciar servidores.
  * **[Amazon EC2 (Elastic Compute Cloud)](https://aws.amazon.com/pt/ec2/)**: Servidor virtual utilizado para a computação pesada e de longa duração. É aqui que os algoritmos de visão computacional são executados.
  * **[Amazon EBS (Elastic Block Store)](https://aws.amazon.com/pt/ebs/)**: Fornece um volume de armazenamento em bloco de alta performance, anexado à instância EC2, para persistir o software de análise, bibliotecas e dados temporários.

## 🚀 Como Usar

Para clonar este repositório e visualizar os arquivos:

```bash
git clone https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
```

