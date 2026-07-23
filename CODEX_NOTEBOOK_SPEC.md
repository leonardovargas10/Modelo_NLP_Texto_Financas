# INSTRUÇÕES PARA GERAÇÃO DE NOTEBOOK — BENCHMARK DE NLP (ESCOPO REDUZIDO)

Gere um único arquivo Jupyter Notebook chamado:

```text
benchmark_nlp_sentimento_financeiro.ipynb
```

O notebook deverá implementar um benchmark de classificação de sentimento financeiro utilizando o dataset:

```text
Financial PhraseBank Portuguese Translation
```

O pipeline principal deverá ser:

```text
texto original em inglês
→ tradução automática para o português
→ classificação de sentimento
```

Utilize:

- a coluna original em inglês, normalizada internamente para `text_en`
- `text_pt`
- `y`

Crie `text_pt_mt`, contendo a tradução automática de `text_en` para o português. O nome original da coluna em inglês deverá ser detectado e validado durante a leitura; depois, renomeie-a para `text_en`.

Classes:

```text
negative
neutral
positive
```

---

# Regras Gerais

1. Gerar diretamente um arquivo `.ipynb` válido.
2. Utilizar PyTorch para Deep Learning.
3. Utilizar Scikit-Learn para TF-IDF e Regressão Logística.
4. Utilizar LightGBM utilizando embeddings contextuais extraídos do BERTimbau.
5. Utilizar Hugging Face Transformers.
6. Utilizar PEFT para LoRA.
7. Utilizar Optuna apenas para otimizar a LSTM.
8. Definir `SEED = 42`.
9. Utilizar exatamente os mesmos splits em todos os experimentos.
10. Não utilizar o conjunto de teste durante treinamento ou seleção de modelos.
11. Evitar data leakage.
12. Utilizar F1-score Macro como métrica principal.
13. Manter código modular, comentado e didático.
14. Priorizar simplicidade em vez de testar dezenas de alternativas.
15. Traduzir o inglês para o português com um único modelo de tradução previamente treinado e congelado.
16. A tradução não poderá utilizar os rótulos nem ser ajustada sobre os conjuntos do benchmark.
17. Gerar a tradução uma única vez, armazená-la com um identificador estável e reutilizá-la em todos os modelos.
18. Usar os mesmos índices de treino, validação e teste antes e depois da tradução.
19. Tratar `text_pt_mt` como entrada principal dos classificadores.
20. Usar `text_pt` como cenário de controle para medir a degradação introduzida pela tradução automática.
21. Nunca escolher modelos ou hiperparâmetros com base no teste, inclusive na comparação entre os dois cenários.
22. Reportar separadamente o tempo de tradução, a inferência do classificador e o tempo ponta a ponta.

---

# Modelos do Benchmark

Implementar apenas:

1. Regressão Logística + TF-IDF
2. BiLSTM treinada do zero e otimizada com Optuna
3. Embeddings Contextuais (BERTimbau) + LightGBM
4. BERTimbau + LoRA

Não implementar:

- Fine-Tuning completo
- Retrieval-Augmented Classification
- RAG
- Comparações entre RNN/GRU/LSTM
- Comparações entre diversos schedulers
- Comparações entre diversos otimizadores
- Comparações entre vários poolings

---

# Estrutura do Notebook

1. Problema de Negócio
2. Bibliotecas e Ambiente
3. Funções Auxiliares
4. Leitura e Validação dos Dados
5. Separação dos Dados
6. Tradução Automática do Inglês para o Português
7. Avaliação e Validação das Traduções
8. Análise Exploratória
9. Pré-processamento
10. TF-IDF + Regressão Logística
11. BiLSTM treinada do zero
12. Otimização da BiLSTM com Optuna
13. Extração de Embeddings Contextuais (BERTimbau)
14. LightGBM sobre Embeddings
15. Transformer com LoRA
16. Análise de Erros
17. Comparação Consolidada
18. Trade-offs
19. Conclusões

---

# Diretrizes da Tradução

Utilizar um único modelo de tradução inglês-português disponível no Hugging Face Transformers.

O modelo deverá:

- permanecer congelado;
- operar sem acesso a `y`;
- traduzir todos os exemplos com uma configuração fixa e reproduzível;
- preservar, tanto quanto possível, números, percentuais, moedas, nomes próprios e negações;
- produzir `text_pt_mt`, armazenado em cache com o identificador do exemplo, `text_en`, `text_pt` e `y`.

Registrar o nome e a versão do modelo, os parâmetros de geração, o tamanho máximo de sequência, o hardware e o tempo de tradução.

Não selecionar traduções manualmente nem escolher entre vários tradutores com base no desempenho do conjunto de teste.

Comparar `text_pt_mt` com `text_pt` por meio de verificações automáticas e inspeção qualitativa. Métricas de similaridade de tradução podem ser apresentadas como diagnóstico secundário, mas não substituem a métrica principal de classificação.

---

# Diretrizes por Modelo

## TF-IDF + Regressão Logística

Pipeline:

Texto → Limpeza básica → TF-IDF (1-2 grams) → Regressão Logística

Utilizar apenas uma configuração principal com pequenos ajustes em:
- C
- max_features
- class_weight (quando necessário)

Interpretar os coeficientes mais relevantes.

---

## BiLSTM Treinada do Zero

Pipeline:

Texto → Tokenização → Vocabulário → Embedding → BiLSTM → Dropout → Linear

Utilizar obrigatoriamente:

- BiLSTM
- Adam
- CrossEntropyLoss
- Weight Decay
- Gradient Clipping
- Early Stopping

Não comparar arquiteturas ou poolings.

---

## Optuna

Otimizar apenas:

- embedding_dim
- hidden_dim
- dropout
- learning_rate
- weight_decay
- batch_size
- num_layers

Manter fixos:

- BiLSTM
- Adam
- estados ocultos finais
- CrossEntropyLoss

---

## Embeddings + LightGBM

Extrair embeddings utilizando o mesmo backbone:

```text
neuralmind/bert-base-portuguese-cased
```

Congelar o Transformer.

Treinar apenas o LightGBM.

Realizar apenas ajustes moderados de hiperparâmetros.

---

## Transformer + LoRA

Utilizar o mesmo BERTimbau empregado na extração de embeddings.

Configuração sugerida:

- Rank = 8
- Alpha = 16
- Dropout = 0.10
- AdamW
- Warmup
- Early Stopping

Treinar apenas as matrizes LoRA e a cabeça de classificação.

---

# Avaliação

Todos os modelos deverão utilizar exatamente as mesmas métricas nos dois cenários (`text_pt` e `text_pt_mt`):

- Accuracy
- Precision Macro
- Recall Macro
- F1-score Macro
- F1 por classe
- Log Loss
- Matriz de Confusão
- Tempo de treinamento
- Tempo de inferência
- Quantidade de parâmetros treináveis
- Variação do F1-score Macro entre o português de referência e o português traduzido
- Tempo de tradução
- Tempo total do pipeline ponta a ponta

---

# Análise de Erros

Comparar os quatro modelos analisando:

- negação
- textos ambíguos
- números
- percentuais
- linguagem financeira
- confusão entre classes
- omissões, adições e alterações introduzidas pela tradução
- preservação de negações, números, percentuais, moedas e termos financeiros
- mudanças de previsão entre `text_pt` e `text_pt_mt`

Selecionar exemplos representativos de acertos e erros.

---

# Comparação Final

Comparar:

- TF-IDF + Regressão Logística
- BiLSTM + Optuna
- Embeddings + LightGBM
- BERTimbau + LoRA

Responder:

- Qual apresentou melhor desempenho?
- Qual apresentou melhor custo-benefício?
- Quando utilizar cada abordagem?
- Quais limitações foram observadas?
- Quanto desempenho foi perdido ou ganho ao substituir a tradução de referência pela tradução automática?
- Quais erros vieram do tradutor e quais vieram do classificador?
- Qual modelo foi mais robusto aos ruídos de tradução?

---

# Filosofia do Projeto

Este projeto tem como objetivo aprender os principais pipelines modernos de NLP.

Cada abordagem deverá utilizar uma metodologia consolidada e suficientemente representativa, evitando uma quantidade excessiva de experimentos.

O foco será compreender as diferenças entre as representações textuais, o impacto da tradução automática e os trade-offs entre desempenho, custo computacional e interpretabilidade, produzindo um benchmark limpo, didático e reproduzível.
