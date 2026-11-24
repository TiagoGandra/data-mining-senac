# Projeto da disciplina: Voz do Cliente

**Mineração de Textos · Pós-Graduação em Data Science e Inteligência Artificial · Faculdade Senac DF · 2026**
Docente: Prof. Rogério Lopes

## O problema

Vocês são o time de dados de um grande e-commerce. Todo mês chegam dezenas de milhares de avaliações de clientes, e ninguém lê todas. A diretoria quer respostas para três perguntas:

1. **Do que os clientes estão reclamando?** Quais são os temas, e quais crescem.
2. **O problema é do produto ou da loja?** Defeito de fábrica vai para o fornecedor; atraso na entrega, troca e atendimento são responsabilidade da loja.
3. **O que fazer com cada reclamação?** Qual é a urgência, qual setor resolve e, como bônus, que direito do consumidor está em jogo.

Ao longo de três semanas, cada grupo constrói um pipeline que responde a essas perguntas: do texto bruto até um LLM que devolve uma ficha estruturada e avaliada de cada reclamação.

## O dataset

**B2W-Reviews01**: 132.373 avaliações reais de produtos da Americanas.com, publicadas entre janeiro e maio de 2018.

| Campo | O que é |
|---|---|
| `review_title`, `review_text` | Título e texto livre da avaliação |
| `overall_rating` | Nota de 1 a 5 estrelas |
| `recommend_to_a_friend` | Recomendaria a um amigo? (Yes/No) |
| `site_category_lv1`, `site_category_lv2` | Categoria do produto (55 categorias no primeiro nível) |
| `product_name`, `product_brand` | Produto e marca |
| `reviewer_birth_year`, `reviewer_gender`, `reviewer_state` | Perfil do avaliador (pseudonimizado) |

- **Fonte:** https://github.com/americanas-tech/b2w-reviews01
- **Leitura direta no Colab:** `pd.read_csv("https://raw.githubusercontent.com/americanas-tech/b2w-reviews01/main/B2W-Reviews01.csv")`
- **Licença:** CC BY-NC-SA 4.0. Uso acadêmico permitido, com crédito à B2W Digital.
- **Citação:** Real, L.; Oshiro, M.; Mafra, A. *B2W-Reviews01: an open product reviews corpus*. STIL 2019.

Grupos que quiserem usar outro corpus público em português (mínimo de 5.000 documentos) devem pedir aprovação até 18/11.

**Cuidado com dados pessoais (LGPD):** os perfis estão pseudonimizados e não devem ser cruzados com outras fontes. Alguns textos trazem nomes, telefones ou e-mails; na Etapa 3, removam esses dados antes de enviar qualquer texto a uma API externa.

## Formato

- Grupos de 1 a 3 pessoas, definidos até 18/11.
- Um repositório Git por grupo, com README, notebooks e uma pasta de prompts.
- Cada etapa tem uma parte em grupo e uma **parte individual**: cada pessoa entrega a sua, e qualquer membro pode ser chamado a explicar qualquer decisão do grupo.
- **Uso de IA para escrever código é permitido**, com duas condições: declarar no README onde foi usada e saber explicar o que o código faz.

## As quatro etapas

### Etapa 1: Do texto ao vetor
**Laboratório 18/11 · entrega até 23/11, 23h59**

Em grupo:
1. Amostrar o dataset e justificar a amostra. **A amostra é do grupo:** semente diferente de 42 e tamanho diferente de 20 mil (os valores usados em aula); o critério (aleatória, estratificada por nota ou por categoria) é livre, desde que justificado.
2. Análise exploratória: distribuição de notas, comprimento dos textos, vazios e duplicados.
3. **Catálogo de sujeira:** ler 30 avaliações e registrar os problemas encontrados (abreviações, caixa alta, erros, emojis, dados pessoais).
4. **Pipeline de pré-processamento** com cada decisão justificada, e um exemplo de algo que a limpeza apagaria e não deveria.
5. Representar os textos com **TF-IDF** e com **embeddings**.
6. **Busca:** para três consultas (por exemplo, "o produto não chegou"), comparar os 5 resultados mais próximos com TF-IDF e com embeddings.
7. **Individual:** calcular à mão o TF-IDF de três termos em uma avaliação do corpus, usando os DFs reais da amostra, e explicar em um parágrafo uma decisão de pré-processamento do grupo. Cada integrante responde na sua subseção do Item 7 do notebook.

A seção **Etapa 1** do Notebook 01 traz os 7 itens nesta ordem, cada um com instruções, uma célula de código comentada e uma célula para as respostas.

### Etapa 2: Classificar e agrupar
**Laboratório 25/11 · entrega até 30/11, 23h59**

O grupo faz as duas tarefas e pode dividir o trabalho entre os integrantes:

- **Classificar:** prever se o cliente recomendaria o produto, comparando TF-IDF e embeddings com Regressão Logística, com métricas por classe, comparação com o baseline e um limiar justificado pelo custo dos erros.
- **Agrupar:** descobrir os temas das avaliações negativas (notas 1 e 2) com **K-Means** e **LDA**, justificar o número de grupos de cada método, nomear cada tema, dizer se ele é da loja ou do produto e comparar os dois métodos.

Análise de erro: 5 erros do classificador e 5 avaliações mal agrupadas, classificados em categorias (rótulo duvidoso, ironia, texto ambíguo, assunto novo etc.), com uma ação para a categoria mais frequente.

Individual: escolher 3 erros e propor, para cada um, uma ação concreta de melhoria.

A seção **Etapa 2** do Notebook 02 traz os 5 itens do checklist (amostra 1 ponto, classificação 3, agrupamento 3, análise de erro 3 e individual 5), cada um com instruções, células de código comentadas e células para as respostas. A amostra é a mesma do grupo na Etapa 1.

### Etapa 3: LLM no pipeline
**Laboratório 02/12 · entrega até 07/12, 23h59**

1. Definir um **schema Pydantic** para a ficha de cada reclamação. Mínimo: responsável (produto ou loja), aspecto (entrega, produto, atendimento, preço, troca ou outro), urgência de 1 a 5 e resumo em uma frase.
2. Usar um LLM com **saída estruturada** para preencher a ficha de avaliações negativas, com tratamento de erros de validação.
3. **Conjunto de avaliação:** rotular à mão 30 avaliações e medir o acerto do LLM; comparar com o modelo da Etapa 2 quando fizer sentido.
4. Estimar **custo e tempo** para processar 100 mil avaliações.

LLM da disciplina: **API do Gemini**, na cota gratuita do Google AI Studio. Cada pessoa cria a própria chave em aistudio.google.com e a guarda nos *Secrets* do Colab. Chave nunca vai para o repositório. A cota gratuita é limitada por dia: testem com poucas avaliações e guardem as respostas em cache.
5. **Bônus (até 1 ponto):** RAG sobre o Código de Defesa do Consumidor, indicando o artigo aplicável a cada reclamação, com a fonte citada.

Individual: revisar 10 fichas geradas pelo LLM e registrar acertos, erros e alucinações.

### Etapa 4: Apresentação
**08/12 e 09/12**

12 minutos de apresentação mais 3 de perguntas. Problema, decisões, demonstração ao vivo, resultados, erros e limitações. A demonstração vale mais que os slides.

## Avaliação

| Componente | Peso |
|---|---|
| Etapa 1 (grupo 10 + individual 5) | 15% |
| Etapa 2 (grupo 10 + individual 5) | 15% |
| Etapa 3 (grupo 10 + individual 5) | 15% |
| Entrega final: repositório e relatório | 25% |
| Apresentação e demonstração | 30% |

Critérios da entrega final e da apresentação:

| Critério | Peso |
|---|---|
| Correção técnica de cada componente | 30% |
| Justificativa das escolhas (pré-processamento, modelo, schema, prompt) | 20% |
| Análise de erro e leitura crítica dos resultados | 20% |
| Organização do repositório (README, prompts versionados, reprodutível) | 15% |
| Comunicação e demonstração ao vivo | 15% |

## Calendário

| Data | Encontro | Projeto |
|---|---|---|
| 17/11 | Conceitual: do texto ao vetor | Apresentação do projeto |
| 18/11 | Laboratório | Etapa 1 |
| 24/11 | Conceitual: classificação e agrupamento | Entrega da Etapa 1 (23/11) |
| 25/11 | Laboratório | Etapa 2 |
| 01/12 | Conceitual: LLMs no pipeline | Entrega da Etapa 2 (30/11) |
| 02/12 | Laboratório | Etapa 3 |
| 08/12 | Apresentações | Entrega da Etapa 3 (07/12) |
| 09/12 | Apresentações | |
