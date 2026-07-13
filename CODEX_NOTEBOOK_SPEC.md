# INSTRUÇÕES PARA GERAÇÃO DE NOTEBOOK — BENCHMARK DE NLP (ESCOPO REDUZIDO)

Gere um único arquivo Jupyter Notebook chamado:

```text
benchmark_nlp_sentimento_financeiro.ipynb
```

O notebook deverá implementar um benchmark de classificação de sentimento financeiro utilizando o dataset:

```text
Financial PhraseBank Portuguese Translation
```

Utilize apenas:

- `text_pt`
- `y`

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
6. Análise Exploratória
7. Pré-processamento
8. TF-IDF + Regressão Logística
9. BiLSTM treinada do zero
10. Otimização da BiLSTM com Optuna
11. Extração de Embeddings Contextuais (BERTimbau)
12. LightGBM sobre Embeddings
13. Transformer com LoRA
14. Análise de Erros
15. Comparação Consolidada
16. Trade-offs
17. Conclusões

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

Todos os modelos deverão utilizar exatamente as mesmas métricas:

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

---

# Análise de Erros

Comparar os quatro modelos analisando:

- negação
- textos ambíguos
- números
- percentuais
- linguagem financeira
- confusão entre classes

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

---

# Filosofia do Projeto

Este projeto tem como objetivo aprender os principais pipelines modernos de NLP.

Cada abordagem deverá utilizar uma metodologia consolidada e suficientemente representativa, evitando uma quantidade excessiva de experimentos.

O foco será compreender as diferenças entre as representações textuais e os trade-offs entre desempenho, custo computacional e interpretabilidade, produzindo um benchmark limpo, didático e reproduzível.
