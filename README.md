# RAG para Análise de Contratos

Pipeline de **Retrieval-Augmented Generation (RAG)** que permite fazer perguntas em linguagem natural sobre documentos contratuais e receber respostas precisas, baseadas apenas no conteúdo real do documento — sem alucinações do modelo.

## 💡 O problema

Encontrar uma cláusula específica em um contrato longo (reajuste, vigência, multa, rescisão) costuma exigir busca manual e leitura repetida. Este projeto automatiza isso: o usuário pergunta em português comum, e o sistema retorna a resposta com base no trecho exato do contrato.

## ⚙️ Como funciona

1. **Chunking** — o documento é dividido em pedaços menores com sobreposição (`RecursiveCharacterTextSplitter`), preservando contexto entre os trechos.
2. **Embeddings** — cada chunk é transformado em vetor semântico usando `sentence-transformers/all-mpnet-base-v2` (768 dimensões).
3. **Banco vetorial** — os vetores são armazenados no **Chroma**, permitindo busca por similaridade.
4. **Busca semântica** — a pergunta do usuário é convertida em vetor e comparada aos chunks armazenados, retornando os trechos mais relevantes.
5. **Geração da resposta** — os trechos recuperados são enviados como contexto para o **LLaMA 3.3 70B** (via Groq), que gera a resposta final com base apenas nesse contexto.

```
Pergunta → Embedding → Busca no banco vetorial → Chunks relevantes → LLM → Resposta
```

## 🛠️ Stack

| Componente        | Tecnologia                                      |
|-------------------|--------------------------------------------------|
| Orquestração       | LangChain (LCEL)                                 |
| LLM                | Groq — LLaMA 3.3 70B Versatile                   |
| Embeddings         | HuggingFace — sentence-transformers/all-mpnet-base-v2 |
| Banco vetorial     | Chroma                                           |
| Chunking           | RecursiveCharacterTextSplitter                   |

## 🚀 Como rodar

1. Clone o repositório e abra o notebook no Google Colab ou Jupyter.
2. Instale as dependências (primeira célula do notebook):
   ```bash
   pip install langchain langchain-groq langchain-huggingface langchain-community langchain-text-splitters chromadb sentence-transformers tiktoken
   ```
3. Configure sua chave da API Groq como variável de ambiente:
   ```python
   import os
   os.environ["GROQ_API_KEY"] = "sua_chave_aqui"  # obtenha em console.groq.com
   ```
4. Substitua o documento de exemplo pelo seu próprio contrato (texto simples).
5. Execute as células na ordem — a última célula faz uma pergunta de teste sobre o contrato.

## 📌 Exemplo

```
Pergunta: Qual é a cláusula de reajuste?
Resposta: O contrato prevê reajuste anual pelo IPCA, calculado em dezembro
e aplicado em janeiro do ano seguinte...
```

## 🔮 Possíveis evoluções

- Upload de PDF/DOCX real em vez de texto colado manualmente
- Interface web (Streamlit ou Gradio) para uso sem precisar rodar o notebook
- Suporte a múltiplos documentos e comparação entre contratos
- Histórico de conversas (memória) para perguntas de acompanhamento

## 🎓 Origem do projeto

Este projeto foi desenvolvido a partir das lives **"Incomparáveis"**, da **Xperiun**, ministradas por **Iago Braz**, como prática dos conceitos de RAG (Retrieval-Augmented Generation) ensinados na série.  

## 👩‍💻 Autora

Desenvolvido por [Tissiany Delmiro](https://github.com/TissianyDelmiro) como parte de estudos em Agentes de IA e RAG.
