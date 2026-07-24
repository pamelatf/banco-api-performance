# 🏦 banco-api-performance

Testes de performance da API do Banco utilizando **k6** e **JavaScript**.

## 📖 Introdução

Este repositório contém uma suíte de testes de performance desenvolvida para avaliar o comportamento da **API do Banco** sob carga. Os testes simulam operações reais do sistema — como autenticação de usuários e transferências bancárias — permitindo medir tempos de resposta, taxa de falhas e a capacidade da API de sustentar múltiplos usuários virtuais simultâneos.

O projeto foi estruturado de forma modular, separando dados de teste, configurações, funções auxiliares e scripts de teste, o que facilita a manutenção e a criação de novos cenários.

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Descrição |
|------------|-----------|
| [k6](https://k6.io/) | Ferramenta open source para testes de carga e performance |
| [JavaScript (ES6+)](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript) | Linguagem utilizada na escrita dos scripts de teste |
| [Web Dashboard do k6](https://grafana.com/docs/k6/latest/results-output/web-dashboard/) | Acompanhamento dos resultados em tempo real e exportação de relatório em HTML |

## 📂 Estrutura do Repositório

```
banco-api-performance/
├── config/
│   └── config.local.json        # Configurações locais (ex.: baseUrl da API)
├── fixtures/
│   └── postLogin.json           # Massa de dados para a requisição de login
├── helpers/
│   └── autenticacao.js          # Função reutilizável para obtenção do token de autenticação
├── tests/
│   ├── login.test.js            # Teste de performance do endpoint de login
│   └── transferencias.test.js   # Teste de performance do endpoint de transferências
├── utils/
│   └── variaveis.js             # Funções utilitárias (ex.: resolução da URL base)
└── README.md
```

## 🎯 Objetivo de Cada Grupo de Arquivos

| Grupo | Objetivo |
|-------|----------|
| **`config/`** | Centraliza as configurações do ambiente, como a `baseUrl` da API utilizada quando nenhuma variável de ambiente é informada. |
| **`fixtures/`** | Armazena a massa de dados (payloads) enviada nas requisições, como as credenciais de login. |
| **`helpers/`** | Contém funções de apoio reutilizáveis entre os testes, como a autenticação e obtenção do token de acesso. |
| **`tests/`** | Reúne os scripts de teste de performance propriamente ditos, com a definição de cenários de carga (`stages`), critérios de aceitação (`thresholds`) e validações (`checks`). |
| **`utils/`** | Agrupa funções utilitárias genéricas, como a leitura da variável de ambiente da URL base com fallback para a configuração local. |

## 📥 Modo de Instalação

1. **Instale o k6** na sua máquina, conforme o seu sistema operacional ([documentação oficial](https://grafana.com/docs/k6/latest/set-up/install-k6/)):

   ```bash
   # Windows (winget)
   winget install k6 --source winget

   # macOS (Homebrew)
   brew install k6

   # Linux (Debian/Ubuntu)
   sudo gpg -k
   sudo gpg --no-default-keyring --keyring /usr/share/keyrings/k6-archive-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C5AD17C747E3415A3642D57D77C6C491D6AC1D69
   echo "deb [signed-by=/usr/share/keyrings/k6-archive-keyring.gpg] https://dl.k6.io/deb stable main" | sudo tee /etc/apt/sources.list.d/k6.list
   sudo apt-get update
   sudo apt-get install k6
   ```

2. **Clone o repositório**:

   ```bash
   git clone https://github.com/pamelatf/banco-api-performance.git
   cd banco-api-performance
   ```

3. **Configure a URL base da API** no arquivo `config/config.local.json` (opcional, usada como fallback):

   ```json
   {
       "baseUrl": "http://localhost:3000"
   }
   ```

## ▶️ Modo de Execução

### Execução básica

Execute um teste informando a URL base da API através da variável de ambiente `BASE_URL`:

```bash
k6 run tests/login.test.js -e BASE_URL=http://localhost:3000
```

> 💡 Caso a variável `BASE_URL` não seja informada, será utilizado o valor de `baseUrl` definido em `config/config.local.json`.

### Execução com relatório em tempo real e exportação em HTML

O k6 disponibiliza um **Web Dashboard** para acompanhar a execução do teste em tempo real pelo navegador (em `http://localhost:5665`) e exportar o relatório final em HTML ao término da execução. Para isso, utilize as variáveis de ambiente do próprio k6:

```bash
K6_WEB_DASHBOARD=true K6_WEB_DASHBOARD_EXPORT=html-report.html k6 run tests/login.test.js -e BASE_URL=http://localhost:3000
```

| Variável | Função |
|----------|--------|
| `K6_WEB_DASHBOARD=true` | Habilita o dashboard web para acompanhamento do teste em tempo real |
| `K6_WEB_DASHBOARD_EXPORT=html-report.html` | Exporta automaticamente o relatório em HTML ao final da execução |
| `BASE_URL` | Define a URL base da API que será testada |

> 💻 **No Windows (PowerShell)**, defina as variáveis antes do comando:
>
> ```powershell
> $env:K6_WEB_DASHBOARD="true"; $env:K6_WEB_DASHBOARD_EXPORT="html-report.html"; k6 run tests/login.test.js -e BASE_URL=http://localhost:3000
> ```

---

Desenvolvido por [Pâmela T. Fagundes](https://github.com/pamelatf) 💜
