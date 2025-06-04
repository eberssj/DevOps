
# Guia de Testes Unitários no DevOps 🧪

## 1. Introdução aos Testes Unitários 🌟
Testes unitários são verificações automatizadas que validam o comportamento de unidades individuais de código, como funções, métodos ou componentes, de forma isolada. No contexto de DevOps, eles são fundamentais para:

- **Confiabilidade ✅**: Identificam erros rapidamente durante o desenvolvimento, reduzindo bugs em produção.
- **Integração Contínua 🔄**: Garantem merges seguros nas branches `develop` e `main`, facilitando a entrega contínua.
- **Redução de Riscos 🛡️**: Validam regras de negócio antes do deploy, assegurando qualidade.

No projeto, os testes unitários cobrem o backend (rotas FastAPI) e o frontend (componentes React/TSX), usando mocks para simular dependências como o banco de dados PostgreSQL. 🗄️

### O que é uma Unidade? 🔍

#### Unidade no Backend (Python/FastAPI) 🐍
Uma **unidade** no backend é definida como:

- **Função independente**: Funções que implementam lógica de negócio específica, como validações, cálculos, transformações de dados ou processamento de regras.
  - *Exemplo*: `validar_cep()`, `calcular_distancia_estacoes()`, `formatar_dados_sensor()`

- **Método de classe**: Métodos que encapsulam comportamentos específicos dentro de uma classe, especialmente aqueles que contêm regras de negócio.
  - *Exemplo*: `Estacao.validar_coordenadas()`, `Sensor.processar_leitura()`, `Usuario.autenticar()`

- **Características importantes**:
  - Deve ser testável de forma isolada (sem dependências externas como banco de dados)
  - Possui entrada e saída bem definidas
  - Implementa uma regra de negócio específica e clara
  - Pode ser mockada quando usada como dependência de outras unidades

#### Unidade no Frontend (React/TSX) ⚛️
Uma **unidade** no frontend é definida como:

- **Componente React individual**: Componentes que possuem lógica própria, estado interno ou comportamentos específicos.
  - *Exemplo*: `BotaoCTA`, `FormularioEstacao`, `CardSensor`, `ModalConfirmacao`

- **Funções auxiliares (utils)**: Funções puras que executam transformações, validações ou cálculos específicos.
  - *Exemplo*: `formatarData()`, `validarEmail()`, `calcularPercentual()`, `ordenarListaPorNome()`

- **Custom Hooks**: Hooks personalizados que encapsulam lógica reutilizável de estado ou efeitos.
  - *Exemplo*: `useEstacoes()`, `useValidacao()`, `useLocalStorage()`

- **Características importantes**:
  - Componentes devem ser renderizáveis independentemente
  - Funções utils devem ser puras (mesma entrada = mesma saída)
  - Deve ter responsabilidade única e bem definida
  - Pode ser testada isoladamente com mocks para props ou dependências externas

### Princípios Gerais 📋
Independente da tecnologia, uma **unidade** deve:
- ✅ Ter uma responsabilidade única e bem definida
- ✅ Ser testável de forma isolada
- ✅ Possuir entrada e saída previsíveis
- ✅ Implementar uma lógica de negócio específica
- ❌ Não depender diretamente de recursos externos (DB, APIs, arquivos)
- ❌ Não misturar múltiplas responsabilidades

## 2. Integração com Gitflow 🌳

O projeto adota o Gitflow com as seguintes branches:

- `main`: Código estável, consolidado ao final de cada sprint. 🏁
- `develop`: Integração e testes de funcionalidades desenvolvidas. 🔧
- `Testes-Unitários`: Branch dedicada exclusivamente para desenvolvimento e manutenção dos testes unitários. 🧪
- **Branches de atividade**: Nomeadas como `[SCRUM-<número>] feat/fix: <mensagem>`, criadas para cada tarefa. 📝

### Fluxo de Testes no Gitflow

**Criação da branch de atividade**:

- Criada a partir de `develop`, e.g., `[SCRUM-1] feat: criar rota de estação`.
- O desenvolvedor implementa a funcionalidade e escreve testes unitários correspondentes.
- Testes são executados localmente para validar os critérios de aceitação. ✅

**Merge para `develop`**:

- A branch é enviada via merge request.
- O pipeline de CI/CD executa os testes automaticamente, verificando cobertura (mínimo 80%, exigência do QA). 🚀
- Se os testes falharem, o merge é bloqueado até correções. 🚫

**Consolidação na `main`**:

- No final da sprint, `develop` é mergeada em `main`.
- Testes são reexecutados para garantir integridade antes do deploy. 🛠️

> **Regra de QA 📋**: O time de QA exige cobertura mínima de 80% nos testes unitários, validada pelo pipeline de CI/CD, para aprovar o deploy.

## 3. Testes Unitários no Backend (FastAPI) 🐍

Os testes do backend utilizam `pytest` e `pytest-asyncio`, com um banco de dados simulado (SQLite em memória) via SQLAlchemy. A configuração está em `tests/conftest.py`, e os testes em arquivos como `tests/test_estacoes.py`.

### Pré-requisitos

**Dependências**:
```
pip install pytest==7.4.0 pytest-asyncio==0.21.0 httpx==0.24.0 sqlalchemy==2.0.0
```

**Ambiente**: Arquivo `tests/conftest.py` configurado com fixtures `db_session` e `client`.

**Sincronia**: Testes (e.g., `tests/test_estacoes.py`) devem corresponder às funcionalidades (e.g., `app/routers/estacoes.py`).

### Comandos para Testes

- **Rodar todos os testes**:
```
pytest tests/ -v
```

- **Rodar teste específico**:
```
pytest tests/test_estacoes.py::test_create_estacao_success -v
```

- **Verificar cobertura**:
```
pytest --cov=app --cov-report=term tests/
```

### Padrão de Escrita

Usamos o padrão **Given-When-Then**:

- **Given**: Configura o estado inicial (mocks, dados simulados).
- **When**: Executa a ação testada (e.g., requisição HTTP).
- **Then**: Verifica o resultado (e.g., status code, dados retornados).

### Estrutura de Arquivos

- Testes em `tests/` (e.g., `tests/test_estacoes.py`).
- Nomeação descritiva, sem seguir a hierarquia do código-fonte.

### Exemplo de Teste Aceito (Backend)

![Captura de tela 2025-05-12 081758](https://github.com/user-attachments/assets/02ec8545-6aab-4567-9fe0-f2a3c3453088)

### Exemplo de Teste Rejeitado (Backend)

![Captura de tela 2025-05-12 094202](https://github.com/user-attachments/assets/26cd4704-96a6-40ab-ae31-eff875d6ba88)


## 4. Testes Unitários no Frontend (React/TSX) ⚛️

Os testes do frontend usam **Jest** com **React Testing Library** para testar componentes como `BotaoCTA`. Configuração em `tests/setupTests.ts` e `jest.config.js`, com testes em `tests/test_BotaoCTA.test.tsx`.

### Pré-requisitos

**Dependências**:
```
npm install --save-dev @testing-library/react@14.0.0 @testing-library/jest-dom@5.16.0 jest@29.7.0 ts-jest@29.1.0 @types/jest@29.5.0
```

**Ambiente**: Configuração em `jest.config.js` e `tests/setupTests.ts`.

**Sincronia**: Testes (e.g., `tests/test_BotaoCTA.test.tsx`) correspondem aos componentes (e.g., `src/components/BotaoCTA.tsx`).

### Comandos para Testes

- **Rodar todos os testes**:
```
npm run test
```

- **Rodar teste específico**:
```
npm run test tests/test_BotaoCTA.test.tsx
```

- **Verificar cobertura**:
```
npm run test -- --coverage
```

### Padrão de Escrita

Usamos o padrão **Setup-Action-Expect**:

- **Setup**: Renderiza o componente e configura o estado.
- **Action**: Simula interações (e.g., cliques).
- **Expect**: Verifica o comportamento ou estado.

### Estrutura de Arquivos

- Testes em `tests/` (e.g., `tests/test_BotaoCTA.test.tsx`).
- Nomeados para refletir o componente testado, com extensão `.test.tsx`.

### Exemplo de Teste Aceito (Frontend)

![Captura de tela 2025-05-14 112056](https://github.com/user-attachments/assets/93f5cd40-2c90-4b33-82fe-8a8abc9dd949)


### Exemplo de Teste Rejeitado (Frontend)

![Captura de tela 2025-05-14 114956](https://github.com/user-attachments/assets/151adf7c-9db5-409a-a3be-c5adfeb23a1f)


## 5. Checklist de Qualidade 📝

Antes de abrir um merge request:

- ✅ Todos os testes unitários passam localmente.
- 🛠️ Novas funcionalidades têm testes para fluxos principais e de erro.
- 📋 Critérios de aceitação (Jira) estão cobertos.
- 🚀 Pipeline de CI/CD executa testes automaticamente.
- 📊 Cobertura de testes ≥ 80% (exigência do QA para deploy).

## 6. Resumo do Workflow de Testes 🛠️

### Como é Testado? 🧪

#### Backend (FastAPI):

- **Rotas testadas**: Funções como `create_estacao`.
- **Ferramentas**: `pytest`, `pytest-asyncio`, `FastAPI TestClient`, `unittest.mock`.
- **Mocks**: Simulam o banco de dados (SQLite em memória) e retornos de modelos (e.g., `Estacao`, `Parametro`).
- **Cenários testados**:
  - **Sucesso**: Criação de estação com dados válidos, retorno de status 201, e validação dos dados retornados.
  - **Erro**: Casos como estação inexistente (404), dados inválidos (422), ou erros internos (500).
  - **Validações**: Formato de CEP, intervalos de latitude/longitude, e associações com sensores.

- **Cobertura**: Usamos `pytest-cov` para garantir 100% de cobertura, incluindo ramificações condicionais. A exigência mínima do QA é 80%.
- **Exemplo**: Um teste para `POST /estacoes/` verifica a criação de uma estação com sensores associados, mockando a sessão do banco e validando o JSON retornado.

#### Frontend (React/TSX):

- **Componentes testados**: Componentes como `BotaoCTA`, verificando renderização e interações.
- **Ferramentas**: `Jest`, `React Testing Library`, `ts-jest`.

## Por Quem é Testado? 👥

### Desenvolvedores:
- Escrevem e executam testes nas branches de atividade (e.g., `feature/add-estacao-route`).
- Garantem 100% de cobertura localmente antes de abrir um PR, usando `pytest` (backend) ou `npm run test` (frontend).
- **Exemplo**: Um desenvolvedor implementa `create_estacao` e escreve `test_create_estacao` em `tests/test_estacoes.py`.

### Revisores:
- Durante o PR para `develop`, outro desenvolvedor revisa o código e os testes, verificando:
  - Cobertura de cenários (sucesso e erro).
  - Conformidade com convenções (e.g., sufixo `_test.py` ou `.test.tsx`).
  - Cobertura mínima de 80% (exigência do QA).

### QA:
- Define a regra de cobertura mínima (80%) e valida os relatórios de cobertura no pipeline.
- Revisa critérios de aceitação (Jira) para garantir que os testes os atendem.

### DevOps:
- Configura e mantém o pipeline de CI/CD (e.g., GitHub Actions, Jenkins) para executar testes automaticamente.
- Garante que o pipeline bloqueia merges se a cobertura for < 80% ou se testes falharem.
- **Automação**: O pipeline executa testes em cada push para `develop` e `main`, garantindo validação contínua.

---

## Quando é Testado? 🕒

### Durante o desenvolvimento:
- Localmente, nas branches de atividade, enquanto o desenvolvedor implementa a funcionalidade.
- **Exemplo**: Executa `pytest tests/test_estacoes.py` ou `npm run test tests/test_BotaoCTA.test.tsx` várias vezes ao dia.

### Antes do merge para develop:
- Ao abrir um PR, o pipeline de CI/CD executa todos os testes automaticamente.
- **Gatilho**: Cada push para o PR ou branch `develop`.
- A cobertura é verificada (mínimo 80%), e o merge é bloqueado se os testes falharem.

### Antes do merge para main:
- No final da sprint, quando `develop` é mergeada em `main`.
- O pipeline reexecuta todos os testes para garantir que o código consolidado está correto.

### Frequência:
- **Contínua**: Testes locais durante o desenvolvimento (várias vezes ao dia).
- **Por evento**: Em cada push/PR para `develop` (diário ou várias vezes por sprint).
- **Sazonal**: No merge para `main` (final da sprint, geralmente quinzenal ou mensal).

