# Mineração de Textos 2026

Material da disciplina **Mineração de Textos** · Pós-Graduação em Data Science e IA · Faculdade Senac DF · Prof. Rogério Lopes

O projeto da disciplina, **Voz do Cliente**, usa 132 mil avaliações reais de clientes, em português, para responder a três perguntas de negócio:

1. **Do que os clientes reclamam?** Temas e tendências.
2. **É do produto ou da operação?** Defeito é do fornecedor; entrega e troca, da loja.
3. **Qual a ação?** Prioridade, área responsável e base legal.

Enunciado completo, critérios e pesos: [`projeto/enunciado.md`](projeto/enunciado.md).

## Notebooks

| Semana | Notebook | Conteúdo | Etapa do projeto |
|---|---|---|---|
| 17 e 18/11 | [`01_do_texto_ao_vetor.ipynb`](notebooks/01_do_texto_ao_vetor.ipynb) · [Abrir no Colab](https://colab.research.google.com/github/rglopesdf/mineracao-textos-2026/blob/main/notebooks/01_do_texto_ao_vetor.ipynb) | Pré-processamento, TF-IDF, embeddings e busca semântica | Etapa 1 · entrega 23/11 |
| 24 e 25/11 | `02_classificar_e_agrupar.ipynb` | Classificação, métricas, agrupamento de temas e análise de erros | Etapa 2 · entrega 30/11 |
| 01 e 02/12 | `03_llm_no_pipeline.ipynb` | LLM com saída estruturada, avaliação, custo e RAG | Etapa 3 · entrega 07/12 |
| 08 e 09/12 | — | Apresentações dos grupos | Etapa 4 |

### Cronograma de liberação

- ✅ **Notebook 01** liberado.
- ⏳ Notebook 02: 24/11.
- ⏳ Notebook 03: 01/12.

Cada notebook segue o roteiro das aulas, em blocos **problema → conceito → solução**, e termina com a seção da etapa correspondente do projeto.

## Dataset

**B2W-Reviews01**: 132.373 avaliações da Americanas.com (jan a mai/2018), com texto livre, nota de 1 a 5, recomendação, categoria e perfil pseudonimizado.

- Fonte: [github.com/americanas-tech/b2w-reviews01](https://github.com/americanas-tech/b2w-reviews01)
- Referência: Real, Oshiro e Mafra, *B2W-Reviews01: an open product reviews corpus*, STIL 2019.
- Licença: CC BY-NC-SA 4.0 (uso não comercial).

O dataset **não** está versionado aqui: os notebooks leem o CSV direto do repositório original.

**Dados pessoais (LGPD):** os perfis não devem ser cruzados com outras fontes. Nomes, telefones e e-mails que aparecem nos textos devem ser removidos antes de qualquer envio a API externa.

## Como rodar

### Google Colab (recomendado)

1. Clique em **Abrir no Colab** na tabela acima, ou no badge no topo de cada notebook.
2. *Ambiente de execução › Alterar o tipo de ambiente de execução › GPU T4*.
3. Rode a primeira célula: ela instala o que faltar e baixa o modelo de português do spaCy.

Para salvar o seu trabalho: *Arquivo › Salvar uma cópia no Drive* ou *Salvar uma cópia no GitHub*.

### Local

```bash
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt jupyter
python -m spacy download pt_core_news_sm
jupyter lab
```

## Regras do projeto

- Grupos de 1 a 3 pessoas, com um repositório próprio por grupo.
- **IA generativa para programar é permitida**, desde que declarada no README do grupo. O grupo responde por cada linha.
- Etapa 3: a chave da API do Gemini fica nos *Secrets* do Colab ou em `.env`. Nunca no repositório.
