# 🧠 Imersão Agentes de IA - Alura + Google Gemini 🚀

Este projeto tem como objetivo a construção de um assistente inteligente para triagem de mensagens de Service Desk utilizando a API Google Gemini. Usando ferramentas como LangChain, Google Generative AI, FAISS e LangGraph, o assistente é capaz de classificar solicitações internas de uma empresa, como reembolsos, solicitações de exceções, entre outros, e direcioná-las para a ação mais adequada.

O fluxo do processo envolve a triagem das mensagens dos usuários, o uso de um modelo para responder automaticamente a perguntas, e, se necessário, a abertura de chamados de suporte.

## 🎯 Objetivo do Projeto

Triagem de solicitações internas de empresas (ex: política de reembolso, solicitações de exceção).

Utilização do Google Gemini para análise de contexto e tomada de decisões em tempo real.

Implementação de um fluxo de decisão que orienta a próxima ação com base na triagem.

## 🚀 Tecnologias Usadas

Python (para execução do código)

Google Gemini API (para respostas generativas de IA)

LangChain (para orquestrar interações entre modelos)

LangGraph (para construir e gerenciar o fluxo de decisão)

FAISS (para indexação e busca de documentos)

## 📋 Funcionalidades

Triagem de Solicitações: O sistema analisa a mensagem e decide se deve ser resolvida automaticamente, se falta informação ou se precisa abrir um chamado.

Processamento de PDFs: O código processa documentos PDF de políticas internas, criando embeddings para consultas eficientes.

RAG (Retrieval-Augmented Generation): Respostas baseadas em documentos internos para fornecer respostas mais precisas.

Fluxo de Decisão: Utiliza um grafo de estados para decidir a ação final após a triagem.

## 📦 Instalação
1. Instalar Dependências

Execute o seguinte comando para instalar todas as dependências necessárias:

```bash
!pip install -q --upgrade langchain langchain-google-genai google-generativeai
!pip install -q --upgrade langchain_community faiss-cpu langchain-text-splitters pymupdf
!pip install -q --upgrade langgraph
```

2. Configuração da API do Google Gemini

Para usar a API do Google Gemini, você precisa configurar a chave de API:

```bash
GOOGLE_API_KEY = userdata.get('GEMINI_API_KEY')
``` 

Substitua GEMINI_API_KEY pelo valor da sua chave de API.

⚙️ Como Usar
1. Triagem de Solicitações

O sistema pode ser utilizado para triagem de mensagens. A função triagem(mensagem: str) faz a análise de uma solicitação com base no prompt configurado.

Exemplo de uso:

```bash
mensagem = "Posso reembolsar a internet?"
resultado = triagem(mensagem)
print(resultado)
```

Isso retornará um JSON com a decisão sobre a ação a ser tomada, a urgência e os campos faltantes.

2. Consulta a Políticas Internas (RAG)

O código carrega documentos PDF contendo políticas internas e permite que o sistema responda perguntas baseadas nesses documentos, utilizando a técnica de Retrieval-Augmented Generation (RAG).

Exemplo de consulta:

```bash
pergunta = "Posso reembolsar cursos ou treinamentos?"
resposta = perguntar_politica_RAG(pergunta)
print(resposta)
```
3. Fluxo de Decisão

O fluxo de decisão é orquestrado através do LangGraph. O grafo de estados define como as mensagens devem ser processadas com base em sua triagem, podendo decidir entre:

AUTO_RESOLVER: Quando a questão é clara e a resposta está nas políticas internas.

PEDIR_INFO: Quando a solicitação é vaga ou faltam informações.

ABRIR_CHAMADO: Quando a questão envolve exceções ou solicitações especiais.

Exemplo de fluxo de decisão:

estado_inicial = {"pergunta": "Quero mais 5 dias de trabalho remoto. Como faço?"}
resposta_final = grafo.invoke(estado_inicial)
print(resposta_final)


Isso irá processar a solicitação e decidir a ação final com base na triagem e no contexto.

## 🛠 Estrutura do Código
Aula 01: Conexão com o Gemini e Triagem

Conexão com a API do Google Gemini usando langchain-google-genai.

Triagem de mensagens utilizando um modelo de IA para decidir se a solicitação pode ser auto-resolvida, precisa de mais informações ou deve abrir um chamado.

Aula 02: Carregamento e Processamento de PDFs

Carregamento de arquivos PDF com PyMuPDFLoader.

Divisão dos documentos em blocos menores para processamento com RecursiveCharacterTextSplitter.

Criação de embeddings usando GoogleGenerativeAIEmbeddings e FAISS para indexação e busca de documentos.

Aula 03: Fluxo de Decisão com LangGraph

Construção do fluxo de decisão usando LangGraph para gerenciar os estados do agente.

Execução do grafo de decisão para processar as mensagens dos usuários, decidir a ação e retornar a resposta.

## 📊 Exemplo de Execução

Abaixo estão alguns exemplos de perguntas que podem ser feitas ao sistema:

```bash
testes = [
    "Posso reembolsar a internet?",
    "Quero mais 5 dias de trabalho remoto. Como faço?",
    "Posso reembolsar cursos ou treinamentos da Alura?",
    "Quantas capivaras tem no Rio Pinheiros?"
]

for msg_teste in testes:
    resposta = grafo.invoke({"pergunta": msg_teste})
    print(f"Pergunta: {msg_teste}")
    print(f"Resposta: {resposta['resposta']}")
    if resposta.get('citacoes'):
        for citacao in resposta['citacoes']:
            print(f" - Documento: {citacao['documento']}, Página: {citacao['pagina']}")
            print(f"   Trecho: {citacao['trecho']}")

```
Este código realizará a triagem das mensagens e exibirá a decisão, urgência, ação final e as citações de documentos relevantes.

## 📝 Contribuindo

Se você deseja contribuir para o projeto, siga os seguintes passos:

Faça um fork do repositório.

Crie uma branch para suas alterações:

```bash
git checkout -b minha-nova-funcionalidade
```

Faça commit das suas alterações:

```bash
git commit -m "Descrição das alterações"
```

Push para o seu repositório:

```bash
git push origin minha-nova-funcionalidade
```

Abra um pull request para o repositório principal.

## 📄 Licença

Este projeto está licenciado sob a MIT License
.

🚀 Obrigado por utilizar a Imersão Agentes de IA!

Fique à vontade para abrir issues ou contribuir com pull requests. Estamos sempre em busca de melhorias! 💡
