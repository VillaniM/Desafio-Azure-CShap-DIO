# 📊 Sistema de Gestão de Recursos Humanos (RH)

**Desafio de Projeto - Trilha .NET com Microsoft Azure | DIO**

Este é um sistema robusto de gestão de recursos humanos que implementa um CRUD completo para funcionários, com auditoria em tempo real através de logs estruturados armazenados no Azure.

---

## 📋 Sumário

- [Visão Geral](#visão-geral)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Funcionalidades](#funcionalidades)
- [Tecnologias e Ferramentas](#tecnologias-e-ferramentas)
- [API REST - Endpoints](#api-rest---endpoints)
- [Configuração e Execução](#configuração-e-execução)
- [Modelo de Dados](#modelo-de-dados)
- [Arquitetura](#arquitetura)

---

## 🎯 Visão Geral

O sistema de RH foi desenvolvido como uma **ASP.NET Core Web API** que permite o gerenciamento completo de funcionários de uma empresa. Todas as operações (criação, leitura, atualização e exclusão) são automaticamente auditadas e registradas em uma **Azure Table Storage**, garantindo rastreabilidade e conformidade com políticas de auditoria.

**Objetivo Principal:** Fornecer uma solução escalável e segura para a gestão de dados de funcionários com auditoria integrada.

---

## 📁 Estrutura do Projeto

```
Desafio-Azure-CShap-DIO/
│
├── Controllers/
│   └── FuncionarioController.cs          # Controlador principal da API REST
│
├── Models/
│   ├── Funcionario.cs                    # Modelo de entidade - Funcionário
│   ├── FuncionarioLog.cs                 # Modelo de log com interface TableEntity
│   └── TipoAcao.cs                       # Enumerador de tipos de ações (Inclusão, Atualização, Remoção)
│
├── Context/
│   └── RHContext.cs                      # DbContext do Entity Framework Core
│
├── Migrations/
│   ├── 20220623031755_Initial.cs         # Migration inicial do banco de dados
│   ├── 20220623031755_Initial.Designer.cs
│   └── RHContextModelSnapshot.cs         # Snapshot do modelo de dados
│
├── Properties/
│   └── launchSettings.json               # Configurações de execução e URLs da aplicação
│
├── Program.cs                            # Configuração central da aplicação
├── appsettings.json                      # Configurações de produção
├── appsettings.Development.json          # Configurações de desenvolvimento
├── trilha-net-azure-desafio.csproj       # Arquivo de projeto .NET
└── README.md                             # Este arquivo
```

### Descrição das Pastas Principais

| Pasta | Descrição |
|-------|-----------|
| **Controllers** | Contém os controladores ASP.NET Core que expõem os endpoints da API REST |
| **Models** | Define as entidades de domínio (Funcionario, FuncionarioLog) e enumeradores (TipoAcao) |
| **Context** | Contém o RHContext (DbContext) que gerencia a comunicação com o banco de dados SQL |
| **Migrations** | Histórico das migrações do Entity Framework para versionamento do schema do banco |

---

## 🚀 Funcionalidades

### Principais Funcionalidades

1. **CRUD Completo de Funcionários**
   - ✅ Criar novo funcionário (POST)
   - ✅ Recuperar funcionário por ID (GET)
   - ✅ Atualizar dados do funcionário (PUT)
   - ✅ Deletar funcionário (DELETE)

2. **Auditoria Automática de Operações**
   - Cada operação realizada é registrada automaticamente
   - Logs armazenados em **Azure Table Storage** para escalabilidade
   - Rastreamento de: Inclusão, Atualização e Remoção de registros
   - Serialização JSON dos dados para análise histórica

3. **Persistência de Dados**
   - Funcionários armazenados em **SQL Database** (banco relacional)
   - Logs de auditoria em **Azure Table Storage** (armazenamento NoSQL)
   - Migrations para controle de versão do schema

4. **Documentação Interativa**
   - Swagger/OpenAPI integrado para exploração da API
   - Testes diretos dos endpoints via interface web

---

## 🛠 Tecnologias e Ferramentas

### Framework e Runtime
- **ASP.NET Core 6.0** - Framework web para construção da API
- **.NET 6.0** - Runtime e ferramentas de compilação

### Banco de Dados
- **Microsoft SQL Server** - Banco de dados relacional (via Entity Framework)
- **Entity Framework Core 6.0.6** - ORM (Object-Relational Mapping)
- **Microsoft.EntityFrameworkCore.SqlServer** - Provider do SQL Server

### Armazenamento em Nuvem
- **Azure Table Storage** - Armazenamento NoSQL para logs
- **Azure.Data.Tables 12.6.0** - SDK do Azure para acesso a tabelas

### Documentação e Testes
- **Swashbuckle.AspNetCore 6.2.3** - Implementação do Swagger/OpenAPI
- **Swagger UI** - Interface interativa para testar endpoints

### Ferramentas de Desenvolvimento
- **Visual Studio Code** ou **Visual Studio**
- **dotnet CLI** - Interface de linha de comando do .NET
- **Entity Framework Core Tools** - Ferramentas para migrações

---

## 🔌 API REST - Endpoints

A API REST fornece os seguintes endpoints para gerenciamento de funcionários:

### Tabela de Endpoints

| Método HTTP | Endpoint | Descrição | Parâmetro | Body |
|-------------|----------|-----------|-----------|------|
| `GET` | `/funcionario/{id}` | Obtém funcionário por ID | `id` (int) | ❌ N/A |
| `POST` | `/funcionario` | Cria novo funcionário | ❌ N/A | `{JSON Funcionario}` |
| `PUT` | `/funcionario/{id}` | Atualiza funcionário existente | `id` (int) | `{JSON Funcionario}` |
| `DELETE` | `/funcionario/{id}` | Deleta funcionário | `id` (int) | ❌ N/A |

### Esquema de Requisição/Resposta - Funcionário

```json
{
  "nome": "Nome funcionario",
  "endereco": "Rua 1234",
  "ramal": "1234",
  "emailProfissional": "email@email.com",
  "departamento": "TI",
  "salario": 1000,
  "dataAdmissao": "2022-06-23T02:58:36.345Z"
}
```

### Exemplos de Respostas

**GET /funcionario/{id}** - Sucesso (200 OK)
```json
{
  "id": 1,
  "nome": "João Silva",
  "endereco": "Rua das Flores, 123",
  "ramal": "2145",
  "emailProfissional": "joao.silva@empresa.com",
  "departamento": "Desenvolvimento",
  "salario": 5500.00,
  "dataAdmissao": "2022-06-23T02:58:36.345Z"
}
```

**POST /funcionario** - Criado com Sucesso (201 Created)
- Retorna o objeto criado com o ID gerado

**PUT /funcionario/{id}** - Sucesso (200 OK)
- Retorna confirmação da atualização

**DELETE /funcionario/{id}** - Sucesso (200 OK)
- Retorna confirmação da exclusão

### Acessando o Swagger

Após iniciar a aplicação, acesse a documentação interativa em:
```
https://localhost:7020/swagger/index.html
```

---

## ⚙️ Configuração e Execução

### Pré-requisitos

- **.NET 6.0 SDK** instalado
- **SQL Server** (local ou remoto)
- **Conta Azure** (para armazenamento de tabelas)
- **Visual Studio Code** ou **Visual Studio 2022**

### Passos para Execução Local

#### 1. Clonar o Repositório
```bash
git clone https://github.com/VillaniM/Desafio-Azure-CShap-DIO.git
cd Desafio-Azure-CShap-DIO
```

#### 2. Restaurar Dependências
```bash
dotnet restore
```

#### 3. Configurar Connection Strings

Editar `appsettings.Development.json`:
```json
{
  "ConnectionStrings": {
    "ConexaoPadrao": "Server=localhost;Database=RHDatabase;User Id=sa;Password=SuaSenha123;",
    "SAConnectionString": "DefaultEndpointsProtocol=https;AccountName=seuarmazenamento;AccountKey=suachave;EndpointSuffix=core.windows.net",
    "AzureTableName": "FuncionarioLogs"
  }
}
```

**Variáveis Necessárias:**
- `ConexaoPadrao` - Connection string do SQL Server
- `SAConnectionString` - Connection string do Azure Table Storage
- `AzureTableName` - Nome da tabela no Azure Table Storage

#### 4. Executar Migrations (se necessário)
```bash
dotnet ef database update
```

#### 5. Compilar o Projeto
```bash
dotnet build
```

#### 6. Executar a Aplicação
```bash
dotnet run
```

A API estará disponível em: `https://localhost:7020`

---

## 📊 Modelo de Dados

### Entidade: Funcionario

Representa um funcionário da empresa com suas informações básicas.

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `Id` | int | Identificador único (chave primária) |
| `Nome` | string | Nome completo do funcionário |
| `Endereco` | string | Endereço residencial |
| `Ramal` | string | Ramal telefônico da empresa |
| `EmailProfissional` | string | Email corporativo |
| `Departamento` | string | Departamento de lotação |
| `Salario` | decimal | Salário mensal |
| `DataAdmissao` | DateTime? | Data de admissão na empresa |

### Entidade: FuncionarioLog (Herança de Funcionario)

Registro de auditoria que herda todos os campos de Funcionário e adiciona informações de rastreamento.

| Campo | Tipo | Descrição |
|-------|------|-----------|
| *(herança)* | - | Todos os campos de Funcionario |
| `TipoAcao` | TipoAcao | Tipo de ação realizada (enum) |
| `JSON` | string | Serialização JSON do funcionário |
| `PartitionKey` | string | Chave de partição no Azure Table |
| `RowKey` | string | Chave de linha (GUID único) |
| `Timestamp` | DateTimeOffset? | Data/hora do registro |
| `ETag` | ETag | Tag de entidade para controle de concorrência |

### Enumerador: TipoAcao

Define o tipo de operação realizada:
```csharp
public enum TipoAcao
{
    Inclusao = 0,      // Novo funcionário criado
    Atualizacao = 1,   // Dados do funcionário atualizados
    Remocao = 2        // Funcionário deletado
}
```

---

## 🏛️ Arquitetura

### Fluxo de Dados

```
Cliente HTTP
    ↓
[ASP.NET Core Controller]
    ↓
┌───────────────────────────────────────┐
│ FuncionarioController                 │
│ - ObterPorId()                        │
│ - Criar()                             │
│ - Atualizar()                         │
│ - Deletar()                           │
└───────────────────────────────────────┘
    ↓                                    ↓
[RHContext - EF Core]          [Azure Table Client]
    ↓                                    ↓
[SQL Database]                  [Azure Table Storage]
    ↓                                    ↓
Dados de Funcionários           Logs de Auditoria
```

### Componentes Principais

1. **ASP.NET Core Web API**
   - Recebe requisições HTTP
   - Valida dados
   - Coordena operações

2. **Entity Framework Core**
   - ORM para SQL Server
   - Gerencia entidades Funcionario
   - Controla migrações de schema

3. **Azure Table Storage**
   - Armazenamento NoSQL escalável
   - Registra todas as mudanças
   - Permite auditoria completa

4. **Swagger/OpenAPI**
   - Documentação automática
   - Interface de teste

### Fluxo de uma Operação (Exemplo: Criar Funcionário)

1. Cliente envia `POST /funcionario` com dados JSON
2. Controller valida e recebe o objeto
3. DbContext insere na tabela Funcionarios
4. `SaveChanges()` persiste no SQL Database
5. FuncionarioLog é criado com tipo "Inclusao"
6. TableClient insere log no Azure Table Storage
7. Resposta 201 Created retorna para o cliente

---

## 🔐 Considerações de Segurança

- Usar **HTTPS** em produção
- Implementar **autenticação e autorização** (JWT/OAuth)
- Validar entrada de dados
- Usar **connection strings seguras** (Azure Key Vault)
- Implementar **rate limiting** e throttling
- Adicionar **logging de erros** centralizado

---

## 📦 Deployment no Azure

### Passos para Deploy

1. **Publicar a aplicação:**
   ```bash
   dotnet publish -c Release -o ./publish
   ```

2. **Criar Azure App Service:**
   - Grupo de recursos
   - Plano de hospedagem (Standard ou Premium)
   - Configure configurações de aplicação

3. **Configurar SQL Database:**
   - Criar servidor e banco de dados
   - Executar migrations no Azure

4. **Configurar Azure Table Storage:**
   - Criar conta de armazenamento
   - Criar tabela para logs

5. **Deploy:**
   - Via Azure Portal, Visual Studio, ou Azure CLI

---

## 🤝 Contribuições

Este é um projeto de desafio educacional da DIO. Para melhorias e correções, faça um fork e abra um pull request.

---

## 📚 Referências

- [Documentação ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/)
- [Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/)
- [Azure Table Storage](https://docs.microsoft.com/en-us/azure/storage/tables/)
- [Swagger/OpenAPI](https://swagger.io/)
