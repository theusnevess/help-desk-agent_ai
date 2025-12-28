# Help Desk Agent AI: Agente de Triagem de Service Desk com LLM

## Descrição do Projeto

Este projeto demonstra a criação de um **Agente de Inteligência Artificial** focado na triagem e classificação automática de chamados de Service Desk. Utilizando um *Large Language Model* (LLM) e técnicas de engenharia de *prompt*, o agente é capaz de analisar a mensagem de um usuário e retornar uma saída estruturada em formato JSON, classificando a solicitação e determinando seu nível de urgência.

O principal diferencial deste projeto é a implementação de um mecanismo de **saída estruturada** (usando `pydantic`), que garante que a resposta do LLM seja sempre um objeto JSON válido e previsível, essencial para a integração em sistemas de *backend* de Service Desk.

## Tecnologias e Bibliotecas

O projeto é construído em Python e utiliza um conjunto de ferramentas modernas para o desenvolvimento de agentes de IA:

| Categoria | Biblioteca/Tecnologia | Uso Principal |
| :--- | :--- | :--- |
| **Modelo de Linguagem** | Google Gemini (`gemini-2.5-flash`) | Processamento de Linguagem Natural e tomada de decisão para triagem. |
| **Framework de Agente** | `langchain` | Orquestração da cadeia de processamento e aplicação de mensagens do sistema. |
| **Validação de Dados** | `pydantic` | Definição e validação do esquema de saída JSON estruturada. |
| **Ambiente** | Python, Jupyter Notebook | Desenvolvimento e execução do código. |

## Estrutura do Agente de Triagem

O agente opera com base em um *prompt* de sistema detalhado que define seu papel e as regras de classificação. A saída é rigidamente controlada para garantir a usabilidade.

### Esquema de Saída Estruturada

A saída do agente é um objeto JSON validado pelo esquema `TriagemOut`, contendo os seguintes campos:

| Campo | Tipo | Descrição | Valores Possíveis |
| :--- | :--- | :--- | :--- |
| `decisao` | String | Ação de triagem recomendada. | `AUTO_RESOLVER`, `PEDIR_INFO`, `ABRIR_CHAMADO` |
| `urgencia` | String | Nível de prioridade da solicitação. | `BAIXA`, `MEDIA`, `ALTA` |
| `campos_faltantes` | Lista de Strings | Informações que o usuário precisa fornecer (somente se `decisao` for `PEDIR_INFO`). | Lista de campos (ex: `tipo de curso`, `valor`) |

### Regras de Decisão

O agente segue as seguintes regras de classificação para o campo `decisao`:

*   **`AUTO_RESOLVER`**: Aplicado a perguntas claras sobre regras ou procedimentos já descritos nas políticas internas da empresa. O agente pode fornecer a resposta diretamente ou indicar que a informação está disponível.
*   **`PEDIR_INFO`**: Aplicado a mensagens vagas ou que não contêm informações suficientes para que o agente possa tomar uma decisão ou fornecer uma resposta completa. O agente deve listar os `campos_faltantes`.
*   **`ABRIR_CHAMADO`**: Aplicado a pedidos que exigem uma ação humana ou aprovação, como solicitações de exceção, liberação de acesso ou aprovação de um procedimento.

## Como Executar o Projeto

Para configurar e executar o agente de triagem, siga os passos abaixo:

1.  **Clone o Repositório:**
    ```bash
    git clone https://github.com/theusnevess/help-desk-agent_ai.git
    cd help-desk-agent_ai
    ```

2.  **Instale as Dependências:**
    Instale as bibliotecas necessárias, incluindo `langchain`, `pydantic` e o SDK do Google Gemini.
    ```bash
    pip install langchain pydantic google-genai python-dotenv
    ```

3.  **Configuração da Chave de API:**
    Crie um arquivo `.env` na raiz do projeto e adicione sua chave de API do Google Gemini:
    ```
    GOOGLE_API_KEY="SUA_CHAVE_AQUI"
    ```

4.  **Execute o Notebook:**
    Abra o arquivo `notebooks/help-desk_agent-ai.ipynb` em um ambiente Jupyter (Jupyter Notebook, JupyterLab ou VS Code) e execute as células sequencialmente para ver o agente em ação e testar diferentes cenários de triagem.
