# INSTRUÇÕES PARA GERAÇÃO DE NOTEBOOK — BENCHMARK DE NLP

Gere um único arquivo Jupyter Notebook chamado:

```text
benchmark_nlp_sentimento_financeiro.ipynb
```

O notebook deverá implementar um benchmark de classificação de sentimento financeiro utilizando o dataset:

```text
Financial PhraseBank Portuguese Translation
```

Fonte:

```text
https://www.kaggle.com/datasets/mateuspicanco/financial-phrase-bank-portuguese-translation
```

Os dados estarão na pasta:

```text
data/
```

Utilize apenas:

* `text_pt`: texto em português brasileiro;
* `y`: classe de sentimento.

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
3. Utilizar scikit-learn para TF-IDF e Regressão Logística.
4. Utilizar LightGBM sobre embeddings densos.
5. Utilizar Hugging Face Transformers para Fine-Tuning.
6. Utilizar PEFT para LoRA.
7. Utilizar Optuna somente para a RNN construída do zero.
8. Definir `SEED = 42`.
9. Utilizar os mesmos splits em todos os experimentos.
10. Não utilizar o conjunto de teste para treinamento, early stopping ou otimização.
11. Evitar leakage entre textos duplicados.
12. Não inventar métricas.
13. Manter código modular, comentado e didático.
14. Não aplicar um único pré-processamento a todos os modelos.
15. Utilizar F1-score macro como métrica principal.

---

# Formatação dos Títulos

Título principal:

```html
# <font color='red' style='font-size: 40px;'> Título da Seção </font>
<hr style='border: 2px solid red;'>
```

Subtítulo:

```html
# <font color='green' style='font-size: 30px;'> Subtítulo </font>
<hr style='border: 2px solid green;'>
```

---

# Modelos do Benchmark

Implementar somente:

1. Regressão Logística com TF-IDF;
2. RNN treinada corretamente do zero;
3. LightGBM com embeddings contextuais;
4. Fine-Tuning completo de um Transformer;
5. LoRA aplicado ao mesmo Transformer;
6. Classificação Aumentada por Recuperação.

O Transformer principal deverá ser um modelo pré-treinado em português, preferencialmente:

```text
neuralmind/bert-base-portuguese-cased
```

---

# Estrutura do Notebook

## 1. Problema de Negócio

Explicar:

* análise de sentimento financeiro;
* objetivo da classificação;
* três classes da variável-alvo;
* aplicação prática;
* motivação do benchmark;
* trade-off entre desempenho, custo e interpretabilidade.

---

## 2. Bibliotecas e Ambiente

Importar apenas o necessário:

```text
pandas
numpy
matplotlib
scikit-learn
torch
transformers
datasets
evaluate
accelerate
peft
sentence-transformers
lightgbm
optuna
captum ou shap
tqdm
joblib
```

Definir seed, dispositivo e diretórios.

Exibir:

* versão do PyTorch;
* versão do Transformers;
* disponibilidade da GPU;
* nome da GPU.

---

## 3. Funções Auxiliares

Criar funções reutilizáveis para:

```python
set_seed()
calcular_metricas()
plot_matriz_confusao()
treinar_rnn()
avaliar_modelo()
extrair_embeddings()
contar_parametros()
medir_tempo_inferencia()
comparar_modelos()
```

Evitar duplicação de código.

---

## 4. Leitura e Validação dos Dados

Carregar apenas:

```text
text_pt
y
```

Verificar:

* dimensões;
* valores ausentes;
* textos vazios;
* duplicidades;
* textos iguais com rótulos diferentes;
* distribuição das classes;
* comprimento dos textos.

Utilizar o seguinte mapeamento:

```python
label2id = {
    "negative": 0,
    "neutral": 1,
    "positive": 2
}
```

---

## 5. Separação dos Dados

Criar divisão estratificada:

```text
70% treino
15% validação
15% teste
```

Salvar os índices dos splits e reutilizá-los em todos os modelos.

Duplicidades não podem aparecer em conjuntos diferentes.

---

## 6. Análise Exploratória

Analisar:

* distribuição das classes;
* número de caracteres;
* número de palavras;
* comprimento por classe;
* palavras mais frequentes;
* bigramas mais frequentes;
* termos financeiros;
* presença de negação;
* números;
* percentuais;
* símbolos monetários.

---

## 7. Pré-processamento de Texto

Manter duas versões:

```text
texto original
texto moderadamente limpo
```

### Para TF-IDF

Avaliar:

* lowercase;
* unigramas e bigramas;
* stopwords;
* preservação de negações;
* preservação de números;
* preservação de percentuais e símbolos monetários.

### Para RNN

Executar:

* tokenização;
* construção do vocabulário;
* tokens `<PAD>` e `<UNK>`;
* conversão para índices;
* padding;
* truncamento;
* Dataset;
* DataLoader.

Definir `max_length` a partir da distribuição real dos textos.

### Para Transformer

Utilizar o tokenizer original do modelo.

Preservar:

* acentuação;
* pontuação;
* números;
* símbolos financeiros;
* texto original.

---

## 8. Regressão Logística com TF-IDF

Explicar resumidamente:

$$
TFIDF(t,d)=TF(t,d)\cdot IDF(t)
$$

Criar pipeline com:

```text
TF-IDF
→
Regressão Logística Multinomial
```

Explorar moderadamente:

```text
ngram_range
min_df
max_df
max_features
sublinear_tf
C
class_weight
```

Apresentar:

* Accuracy;
* Precision macro;
* Recall macro;
* F1-score macro;
* F1 por classe;
* AUC;
* Log Loss;
* Matriz de confusão;
* termos mais importantes por classe.

---

## 9. RNN Treinada do Zero

Construir uma arquitetura recorrente completa utilizando PyTorch.

A arquitetura principal poderá ser uma LSTM ou BiLSTM, mas deverá ser implementada no projeto e treinada do zero.

Fluxo:

```text
Tokens
→
Embedding aprendido do zero
→
LSTM ou BiLSTM
→
Pooling temporal
→
Dropout
→
Camada linear
→
Logits
```

Explicar:

* vocabulário;
* embeddings;
* processamento sequencial;
* estado oculto;
* memória da LSTM;
* padding;
* máscaras;
* pooling;
* dropout;
* camada de saída.

Utilizar:

* `CrossEntropyLoss`;
* Adam ou AdamW;
* weight decay;
* gradient clipping;
* scheduler;
* early stopping.

Não aplicar Softmax dentro do `forward`.

```python
return logits
```

Durante a inferência:

```python
probabilidades = torch.softmax(logits, dim=1)
```

Gerar gráficos de:

* loss de treino;
* loss de validação;
* F1 macro;
* learning rate;
* tempo por época.

---

## 10. Otimização da RNN com Optuna

Otimizar somente a RNN.

Objetivo:

```text
maximizar F1-score macro de validação
```

Espaço inicial:

| Hiperparâmetro | Valores                   |
| -------------- | ------------------------- |
| embedding_dim  | 64, 128, 256              |
| hidden_dim     | 64, 128, 256              |
| num_layers     | 1 a 3                     |
| dropout        | 0,10 a 0,50               |
| learning_rate  | `1e-4` a `3e-3`           |
| weight_decay   | `1e-6` a `1e-3`           |
| batch_size     | 16, 32, 64                |
| gradient_clip  | 0,5 a 5,0                 |
| bidirectional  | True ou False             |
| pooling        | last, mean, max, mean-max |

Utilizar:

* TPE;
* pruning;
* early stopping.

Comparar:

```text
RNN Base
versus
RNN Otimizada
```

---

## 11. Extração de Embeddings Contextuais

Utilizar um modelo de embeddings compatível com português.

Preferir um Sentence Transformer multilíngue ou outro encoder apropriado.

Extrair embeddings para:

* treino;
* validação;
* teste.

Fluxo:

```text
Texto
→
Transformer pré-treinado
→
Embedding denso da sentença
```

Salvar os embeddings em `.npy` ou `.parquet`.

Explicar:

* dimensão do embedding;
* representação semântica;
* diferença entre TF-IDF e embeddings contextuais.

---

## 12. LightGBM com Embeddings

Treinar LightGBM usando os embeddings como features.

Fluxo:

```text
Texto
→
Embedding contextual
→
LightGBM
→
Classe de sentimento
```

Otimizar moderadamente:

```text
num_leaves
max_depth
learning_rate
n_estimators
min_child_samples
subsample
colsample_bytree
reg_alpha
reg_lambda
```

Comparar com:

* Regressão Logística + TF-IDF;
* RNN treinada do zero.

---

## 13. Transformer Pré-Treinado

Utilizar preferencialmente:

```text
neuralmind/bert-base-portuguese-cased
```

Explicar resumidamente:

* tokenização por subpalavras;
* embeddings;
* Self-Attention;
* Query;
* Key;
* Value;
* Multi-Head Attention;
* Feed-Forward;
* conexões residuais;
* Layer Normalization.

Self-Attention:

$$
Attention(Q,K,V)=
softmax\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

Adaptar a cabeça de classificação para três classes.

---

## 14. Fine-Tuning Completo

Realizar Fine-Tuning supervisionado de todos os parâmetros do Transformer.

Configuração inicial:

```text
learning_rate: 2e-5 ou 5e-5
batch_size: 8, 16 ou 32
epochs: 3 a 5
weight_decay: 0.01
warmup_ratio: 0.10
```

Utilizar:

* AdamW;
* scheduler;
* warmup;
* gradient clipping;
* mixed precision;
* gradient accumulation, se necessário;
* early stopping;
* melhor checkpoint por F1 macro.

---

## 15. Fine-Tuning com LoRA

Aplicar LoRA ao mesmo Transformer.

Explicar:

$$
W'=W+\Delta W
$$

$$
\Delta W=BA
$$

Configuração inicial:

| Hiperparâmetro | Valores       |
| -------------- | ------------- |
| rank `r`       | 4, 8, 16      |
| alpha          | 8, 16, 32     |
| dropout        | 0,05 a 0,20   |
| módulos-alvo   | Query e Value |

Comparar com Fine-Tuning completo:

* F1 macro;
* tempo de treinamento;
* memória;
* parâmetros totais;
* parâmetros treináveis;
* tamanho do checkpoint.

---

## 16. Classificação Aumentada por Recuperação

Implementar uma abordagem inspirada em Retrieval-Augmented Classification.

Utilizar apenas textos do conjunto de treino para construir o índice.

Fluxo:

```text
Textos de treino
→
Embeddings
→
Índice vetorial
```

Para cada novo texto:

```text
Texto de validação ou teste
→
Embedding
→
Recuperação dos K textos mais similares
→
Uso dos exemplos recuperados na classificação
```

Implementar uma das seguintes estratégias:

### Estratégia principal

Combinar as probabilidades de um classificador com a distribuição dos rótulos dos vizinhos recuperados:

$$
P_{final}(y|x)
==============

\lambda P_{modelo}(y|x)
+
(1-\lambda)P_{retrieval}(y|x)
$$

Onde:

* $P_{modelo}$ é a probabilidade produzida pelo classificador;
* $P_{retrieval}$ é a distribuição dos rótulos recuperados;
* $\lambda$ controla o peso de cada componente.

O classificador-base poderá ser:

* Transformer;
* Regressão Logística;
* LightGBM.

### Estratégia opcional

Recuperar exemplos semanticamente semelhantes e usá-los como demonstrações em um prompt few-shot.

Cuidados obrigatórios:

* indexar somente o treino;
* nunca indexar validação ou teste;
* evitar textos duplicados;
* não utilizar o rótulo do texto consultado;
* selecionar `K` e `lambda` apenas na validação.

Comparar:

```text
modelo sem recuperação
versus
modelo com recuperação
```

---

## 17. Interpretabilidade e Análise de Erros

### Regressão Logística

Analisar:

* coeficientes;
* palavras importantes;
* bigramas importantes.

### LightGBM

Utilizar:

* feature importance;
* SHAP, quando viável.

### RNN

Utilizar:

* Integrated Gradients;
* importância dos tokens.

### Transformer

Utilizar:

* token attribution;
* Integrated Gradients;
* attention maps como análise complementar.

Analisar erros relacionados a:

* negação;
* contraste entre orações;
* números;
* percentuais;
* expectativas;
* linguagem financeira;
* confusão entre neutro e positivo;
* confusão entre neutro e negativo.

---

## 18. Comparação Consolidada

Criar tabela final contendo:

* modelo;
* representação textual;
* Accuracy;
* Precision macro;
* Recall macro;
* F1 macro;
* F1 por classe;
* AUC;
* Log Loss;
* tempo de treinamento;
* tempo de inferência;
* parâmetros totais;
* parâmetros treináveis;
* memória;
* tamanho do modelo.

Modelos obrigatórios na tabela:

```text
Regressão Logística + TF-IDF
RNN Base
RNN Otimizada
Embeddings + LightGBM
Transformer Fine-Tuning
Transformer LoRA
Classificação Aumentada por Recuperação
```

---

## 19. Trade-offs

Responder:

* TF-IDF foi competitivo?
* A RNN do zero conseguiu aprender com poucos dados?
* A otimização melhorou a RNN?
* Embeddings + LightGBM apresentaram bom custo-benefício?
* Fine-Tuning completo justificou seu custo?
* LoRA alcançou desempenho semelhante treinando menos parâmetros?
* A recuperação melhorou a classificação?
* Qual abordagem é mais simples?
* Qual abordagem é mais interpretável?
* Qual abordagem exige menos memória?
* Qual abordagem oferece o melhor equilíbrio geral?

---

## 20. Conclusões

Responder explicitamente:

* Qual modelo apresentou melhor desempenho?
* Qual apresentou melhor custo-benefício?
* Quando utilizar TF-IDF?
* Quando utilizar uma RNN?
* Quando utilizar embeddings + LightGBM?
* Quando utilizar Fine-Tuning completo?
* Quando utilizar LoRA?
* A recuperação semântica trouxe ganho?
* Quais classes foram mais difíceis?
* Quais limitações foram encontradas?
* Quais são os próximos passos?

---

# Ordem de Execução

```text
1. Leitura, validação e EDA
2. Regressão Logística + TF-IDF
3. RNN Base
4. Optuna na RNN
5. Extração de embeddings
6. Embeddings + LightGBM
7. Fine-Tuning do Transformer
8. LoRA
9. Classificação Aumentada por Recuperação
10. Interpretabilidade
11. Comparação final
12. Conclusões
```

---

# Checklist Final

* [ ] Utilizar apenas `text_pt`
* [ ] Utilizar `y` como target
* [ ] Seed igual a 42
* [ ] Splits iguais para todos os modelos
* [ ] Sem leakage
* [ ] F1 macro como métrica principal
* [ ] Regressão Logística + TF-IDF
* [ ] RNN treinada do zero
* [ ] Optuna somente na RNN
* [ ] Embeddings + LightGBM
* [ ] Fine-Tuning completo
* [ ] LoRA
* [ ] Classificação aumentada por recuperação
* [ ] Interpretabilidade
* [ ] Análise de erros
* [ ] Tabela consolidada
* [ ] Nenhuma métrica inventada
* [ ] Código modular e comentado
