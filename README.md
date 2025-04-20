# Gateway de Pagamento - API Gateway (Next.js)

Este é o Frontend da API Gateway desenvolvido em ***Next.js***.

## Sobre o Projeto

O Gateway de Pagamento é um sistema distribuído composto por:
- Frontend em Next.js (Este repositório)
- [API Gateway em Go com Apache Kafka para comunicação assíncrona](https://github.com/patrick-cuppi/Gateway-Payment-Golang)
- [Sistema de Antifraude em Nest.js](https://github.com/patrick-cuppi/Gateway-Payment-Nest.js)

### Componentes do Sistema

- **Frontend (Next.js)**
  - Interface do usuário para gerenciamento de contas e processamento de pagamentos
  - Desenvolvido com Next.js para garantir performance e boa experiência do usuário

- **Gateway (Go)**
  - Sistema principal de processamento de pagamentos
  - Gerencia contas, transações e coordena o fluxo de pagamentos
  - Publica eventos de transação no Kafka para análise de fraude

- **Apache Kafka**
  - Responsável pela comunicação assíncrona entre API Gateway e Antifraude
  - Garante confiabilidade na troca de mensagens entre os serviços
  - Tópicos específicos para transações e resultados de análise

- **Antifraude (Nest.js)**
  - Consome eventos de transação do Kafka
  - Realiza análise em tempo real para identificar possíveis fraudes
  - Publica resultados da análise de volta no Kafka

## Fluxo de Comunicação

1. Frontend realiza requisições para a API Gateway via REST
2. Gateway processa as requisições e publica eventos de transação no Kafka
3. Serviço Antifraude consome os eventos e realiza análise em tempo real
4. Resultados das análises são publicados de volta no Kafka
5. Gateway consome os resultados e finaliza o processamento da transação

## Ordem de Execução dos Serviços

Para executar o projeto completo, os serviços devem ser iniciados na seguinte ordem:

1. **API Gateway (Go)** - Deve ser executado primeiro pois configura a rede Docker
2. **Serviço Antifraude (Nest.js)** - Depende do Kafka configurado pelo Gateway
3. **Frontend (Next.js)** - Interface de usuário que se comunica com a API Gateway

## Instruções Detalhadas

Cada componente do sistema possui instruções específicas de instalação e configuração em seus repectivos repositórios:

- **Serviço Antifraude**: Consulte o README na pasta do projeto [(clique aqui)](https://github.com/patrick-cuppi/Gateway-Payment-Nest.js).
- **API Gateway em Go**: Consulte o README na pasta do projeto [(clique aqui)](https://github.com/patrick-cuppi/Gateway-Payment-Golang).

## Arquitetura da aplicação
[Visualize a arquitetura completa aqui](https://link.excalidraw.com/readonly/Nrz6WjyTrn7IY8ZkrZHy)

## Pré-requisitos

- [Docker](https://www.docker.com/get-started)

## Importante!

⚠️ **É necessário executar primeiro o serviço Gateway-Payment-Golang** antes deste projeto, pois este frontend utiliza a rede Docker criada pela API em Go.

## Setup do Projeto

1. Clone o repositório:
```bash
git clone https://github.com/patrick-cuppi/Gateway-Payment-Next.js
```

2. Verifique se o serviço Gateway-Payment-Golang já está em execução

3. Inicie os serviços com Docker Compose:
```bash
docker-compose up -d
```

4. Execute a aplicação dentro do container:
```bash
docker-compose exec nextjs bash
npm run dev
```

O frontend estará disponível em `http://localhost:3000`.

## Funcionalidades

### Autenticação
- Login via API Key gerada pelo sistema Gateway
- Proteção de rotas via middleware

### Gerenciamento de Faturas
- Listagem de todas as faturas
- Visualização detalhada de uma fatura específica
- Criação de novas faturas (processamento de pagamento)
- Visualização de status (aprovado, pendente, rejeitado)

## Interface de Usuário

O frontend possui 4 telas principais:

1. **Login** - Entrada da API Key para autenticação
2. **Listagem de Faturas** - Visão geral de todas as transações
3. **Detalhes da Fatura** - Informações completas de uma transação
4. **Criação de Fatura** - Formulário para processamento de um novo pagamento

Todas as telas incluem uma barra de navegação superior que exibe "Gateway Payment" e um botão de logout.

## Integração com API Gateway

O frontend se comunica com a API Gateway para:
- Autenticação de usuários
- Criação e listagem de faturas
- Consulta de detalhes de faturas
- Atualização de dados via revalidação de tags

## Regras de Negócio

- Transações acima de R$ 10.000 são automaticamente enviadas para análise e ficam com status "pendente"
- Transações menores são processadas imediatamente
- A interface mostra status diferenciados por cores: verde (aprovado), amarelo (pendente), vermelho (rejeitado)

## Desenvolvimento

Para desenvolvimento local, você pode editar os arquivos localmente - eles são montados como volume no container Docker, que atualiza automaticamente as mudanças.

## Tecnologias Utilizadas

- Next.js 15 com App Router
- Tailwind CSS para estilização
- TypeScript para tipagem estática
- Server Components e Server Actions
