# Visão Geral
Este projeto implementa um chat que responde perguntas com base no conteúdo de arquivos PDF. O fluxo principal é: extração de texto dos PDFs, fragmentação em trechos, geração de embeddings com Azure OpenAI, indexação em Azure Cognitive Search usando busca vetorial e geração de respostas por um modelo do Azure OpenAI. O objetivo é facilitar revisão de literatura para TCCs ou estudos, oferecendo respostas fundamentadas nos documentos carregados.

# Pré-requisitos
- Conta Azure com permissão para criar recursos (Resource Group, Storage, Cognitive Search).
- Acesso ao Azure Foundry / ambiente fornecido pela sua instituição (terminal e forma de definir variáveis/segredos).
- Python 3.8+ e git (no ambiente Foundry ou local).

Variáveis necessárias (defina como segredos/variáveis no Foundry):
- AZURE_OPENAI_ENDPOINT
- AZURE_OPENAI_KEY
- AZURE_SEARCH_ENDPOINT
- AZURE_SEARCH_API_KEY
- INDEX_NAME (ex.: pdf-index)
- STORAGE_CONNECTION_STRING (opcional, se usar Blob Storage)

# Preparar repositório no Foundry
No Foundry terminal faça:
- Clonar seu repositório (ou criar novo)
<img width="1046" height="265" alt="image" src="https://github.com/user-attachments/assets/71f5f442-1833-42e4-8198-2091f9744781" />
- Colocar PDFs em inputs/
- Faça upload via interface do Foundry para a pasta inputs/ ou use az storage blob upload se usar Blob Storage.

## Configurar segredos / variáveis no Azure Foundry
No painel do Foundry (UI) procure seção de Environment / Secrets e defina:
- AZURE_OPENAI_ENDPOINT = https://seu-openai-endpoint
- AZURE_OPENAI_KEY = sua_chave_openai
- AZURE_SEARCH_ENDPOINT = https://seu-search-endpoint
- AZURE_SEARCH_API_KEY = sua_chave_search
- INDEX_NAME = pdf-index
- STORAGE_CONNECTION_STRING = (se usar)

### Se não houver UI, exporte localmente no terminal (temporário):
<img width="1070" height="335" alt="image" src="https://github.com/user-attachments/assets/18d18513-818c-4c79-97f1-05a9563fbcd5" />

### Instalar dependências (no terminal do Foundry)
<img width="864" height="287" alt="image" src="https://github.com/user-attachments/assets/9520035f-af34-4942-84c6-18e0e6429810" />

# Fluxo de execução (comandos no Foundry):
<img width="1385" height="385" alt="image" src="https://github.com/user-attachments/assets/60be1b02-c49d-4f72-b470-5e62e340d34b" />
<img width="1395" height="362" alt="image" src="https://github.com/user-attachments/assets/98193257-0aad-44dc-b35e-8e47053bb094" />
<img width="1412" height="335" alt="image" src="https://github.com/user-attachments/assets/92aa8aa6-22ce-4965-a20e-adb8c17c92c0" />

---
✨ Dicas simples para iniciantes
- 🧪 Teste com poucos PDFs primeiro — comece com 1–3 arquivos pequenos para entender o fluxo e economizar custo.
- 🎯 Use top 3–5 trechos (top K) como contexto para o modelo; economiza tokens e aumenta a precisão.
- 🔁 Mantenha overlap no chunking (ex.: 100–200 caracteres) para evitar perda de frases entre pedaços.
- 🔍 Verifique os trechos recuperados quando a resposta não estiver clara — o sistema só responde com o que encontra nos PDFs.
- 💸 Monitore custo e uso de API — prefira modelos/embeddings menores na fase de testes.
- 🏷️ Adicionar metadados é útil (autor, título, ano) para permitir filtros e facilitar referências.
- 🔐 Não exponha chaves — guarde segredos/variáveis no gerenciador do Foundry ou em variáveis de ambiente, nunca no repositório.
- 📝 Documente o processo: logs de ingestão, indexação e exemplos de perguntas/respostas enriquecem o portfólio.

---
💡 O que aprendi
- 🧭 A busca vetorial supera palavras-chave
Aprendi que a busca vetorial permite encontrar trechos semanticamente relevantes mesmo quando as perguntas não usam as mesmas palavras dos documentos.
- 🧩 Chunking é crucial para precisão
Percebi que dividir os PDFs em pedaços com overlap melhora muito a recuperação de contexto e evita respostas truncadas.
- ⚖️ Balancear custo e qualidade
Descobri que testar com modelos e embeddings menores economiza crédito sem comprometer a validação inicial do fluxo.
- 🔒 RAG torna as respostas verificáveis
Ao devolver ao modelo apenas trechos recuperados dos PDFs, as respostas ficam mais alinhadas aos documentos e fáceis de checar.
- 🗂️ Metadados e filtros aumentam utilidade prática
Notei que adicionar metadados e permitir filtragem facilita citar fontes e correlacionar ideias durante a revisão de literatura.








