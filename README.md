[← Voltar para Sircoi Geral](https://github.com/joycequoos/Sircoi_Geral)

# 💡 DTEXEC

Anotações e estudo sobre o `dtexec` — ferramenta de linha de comando usada para executar pacotes SSIS (SQL Server Integration Services), com foco em entendimento prático para suporte e desenvolvimento.

## Sumário

- [O que é o dtexec](#o-que-é-o-dtexec)
- [Para que serve](#para-que-serve)
- [Exemplos de uso](#exemplos-de-uso)
- [Onde é usado na prática](#onde-é-usado-na-prática)
- [O dtexec acompanha o SQL Server?](#o-dtexec-acompanha-o-sql-server)
- [dtexec faz parte do SSDT?](#dtexec-faz-parte-do-ssdt)
- [Ciclo de vida de um ETL no SQL Server](#ciclo-de-vida-de-um-etl-no-sql-server)
- [Resumo](#resumo)

---

## O que é o dtexec

**Pergunta:** O que é o `dtexec` que geralmente fica em `C:\Program Files\Microsoft SQL Server\160\DTS\Binn`?

O `dtexec` é uma **ferramenta de linha de comando da Microsoft** usada para **executar pacotes do SQL Server Integration Services (SSIS)**.

Ele geralmente fica no caminho:

<img width="431" height="84" alt="image" src="https://github.com/user-attachments/assets/92274893-6088-4f6e-b757-5f3ead6c2a79" />

> `160` corresponde à versão do SQL Server 2019/2022.

---

## Para que serve

- Executar pacotes SSIS (`.dtsx`) desenvolvidos no **SQL Server Data Tools (SSDT)** ou no **Visual Studio**
- Rodar pacotes armazenados no **MSDB** ou no **SSISDB** (Catálogo do Integration Services)
- Executar pacotes salvos em arquivos (`.dtsx`) no sistema de arquivos
- Permitir passar parâmetros, variáveis e opções de log pela linha de comando

---

## Exemplos de uso

**1. Executar pacote salvo em arquivo:**

<img width="287" height="78" alt="image" src="https://github.com/user-attachments/assets/90fea1df-ee95-4c93-a95a-ee08f118d0be" />

**2. Executar pacote armazenado no Catálogo SSIS (SSISDB):**

<img width="704" height="38" alt="image" src="https://github.com/user-attachments/assets/1f20f8dc-44ef-484e-9b40-d2b854d0088f" />

**3. Passando parâmetros:**

```
dtexec /f "C:\ETL\CargaClientes.dtsx" /SET \Package.Variables[User::DataReferencia].Value;"2025-08-19"
```

---

## Onde é usado na prática

- Em **agendamentos de jobs** no SQL Server Agent
- Em **sistemas externos** que precisam rodar pacotes SSIS (por exemplo, um serviço .NET que chama o `dtexec`)
- Em **scripts de automação** (PowerShell, `.bat`)
- Em **serviços de ETL** (como no SircoiServer, que usa `dtexec` para rodar pacotes SSIS além de procedures SQL)

---

## O dtexec acompanha o SQL Server?

**Pergunta:** O dtexec acompanha o SQL Server?

Sim, o `dtexec` **acompanha o SQL Server**, mas **não em todas as edições**.

### 📌 Onde ele vem incluso

- É instalado junto com o **Integration Services (SSIS)**, que faz parte do **SQL Server Data Tools (SSDT)** e do **SQL Server Integration Services**
- Também aparece ao instalar o **SQL Server Database Engine + Integration Services** no setup do SQL Server
- A versão do `dtexec` depende da versão do SQL Server instalada:

| Versão do dtexec | Versão do SQL Server |
|---|---|
| 130 | SQL Server 2016 |
| 140 | SQL Server 2017 |
| 150 | SQL Server 2019 |
| 160 | SQL Server 2022 |

### 📌 Atenção

- Se você instalar **apenas o SQL Server Database Engine** (sem SSIS), o `dtexec.exe` **pode não estar disponível**
- Também pode vir junto com o **SQL Server Client Tools** (ao instalar os recursos de desenvolvimento)

### 📌 Verificando se o dtexec está instalado

<img width="171" height="58" alt="image" src="https://github.com/user-attachments/assets/2265de4b-aa02-4ab7-9394-9594b0440109" />

ou

<img width="518" height="52" alt="image" src="https://github.com/user-attachments/assets/8d43e7b8-068b-4047-a187-4bfca0962098" />

> 👉 Resumindo: o `dtexec` **vem junto com o SQL Server quando o Integration Services está instalado**. Se não tiver o SSIS, não aparece — mas dá para instalar depois pelo instalador do SQL Server.

---

## dtexec faz parte do SSDT?

**Pergunta:** Então o dtexec faz parte do SQL Server Data Tools (SSDT)?

📌 `dtexec` **não faz parte do SSDT em si**.

- O **SSDT (SQL Server Data Tools)** é a **ferramenta de desenvolvimento** (no Visual Studio) onde você **cria** pacotes SSIS (`.dtsx`)
- Serve para **desenhar e desenvolver** ETLs

📌 O `dtexec` **faz parte do Integration Services (SSIS Runtime)**

- O **Integration Services (SSIS)** é a **engine de execução** que roda os pacotes criados no SSDT
- O `dtexec.exe` é a ferramenta de linha de comando dessa engine
- Vem junto quando se instala o **SQL Server Integration Services** pelo setup (ou os **Client Tools** de SSIS)

### Resumindo a diferença

| Ferramenta | Papel |
|---|---|
| **SSDT** | Desenvolvimento — criar pacotes |
| **dtexec / SSIS** | Execução — rodar pacotes |

Um jeito de pensar:
- Você usa o **SSDT/Visual Studio** para "compilar" seu ETL
- Depois usa o **dtexec** (ou SQL Agent, ou catálogo do SSIS) para **rodar esse ETL em produção**

---

## Ciclo de vida de um ETL no SQL Server

Mapa resumido mostrando o que cada parte (SSDT, SSIS, dtexec, SQL Agent) faz no ciclo de vida completo de um ETL.

### 1. Desenvolvimento — Criando pacotes

**Ferramenta:** SQL Server Data Tools (SSDT), no Visual Studio

O que faz:
- Criar **pacotes SSIS (.dtsx)** com fluxos de dados (ETL)
- Configurar conexões (SQL, arquivos, APIs, etc.)
- Testar localmente no Visual Studio

**Resultado:** arquivos `.dtsx` (ou projeto `.ispac`)

### 2. Implantação — Onde colocar os pacotes

Pacotes podem ser salvos em:
- **Sistema de arquivos** → `.dtsx` direto numa pasta
- **MSDB** → banco do SQL Server
- **SSISDB** (Catálogo do Integration Services) → repositório moderno, com logs e monitoramento

### 3. Execução — Rodando pacotes

**Ferramenta principal:** `dtexec.exe`

O que faz:
- Executa pacotes `.dtsx` ou do catálogo
- Permite passar **parâmetros, variáveis e configs** via linha de comando
- Pode ser chamado manualmente, por script (PowerShell, `.bat`) ou por sistemas externos (ex.: C# via `Process.Start`)

Exemplo:

```
dtexec /f "C:\ETL\CargaClientes.dtsx" /SET \Package.Variables[User::DataReferencia].Value;"2025-08-19"
```

### 4. Agendamento — Automação

**Ferramenta:** SQL Server Agent

O que faz:
- Agenda jobs que rodam pacotes SSIS (internamente chamando o `dtexec`)
- Permite rodar diariamente, semanalmente, etc.
- Pode combinar ETLs com stored procedures, scripts PowerShell, etc.

### 5. Monitoramento & Logs

- **SSISDB** (se usado): oferece relatórios nativos de execução, histórico, falhas
- **Logs customizados**: configurados no pacote ou via parâmetros do `dtexec`
- **Ferramentas externas**: podem capturar saídas do `dtexec` (como o SircoiServer)

---

## Resumo

| Camada | Ferramenta | Papel |
|---|---|---|
| Desenvolvimento | **SSDT** | Onde você desenha o ETL |
| Execução | **SSIS runtime (dtexec)** | Onde você roda o ETL |
| Agendamento | **SQL Agent** | Onde você agenda e automatiza o ETL |
