# Projeto de Benchmark em Processamento de Linguagem Natural

---

# Objetivo

O objetivo deste projeto é realizar um benchmark entre diferentes estratégias de representação e classificação de textos aplicadas à **análise de sentimento financeiro**, comparando desde métodos clássicos baseados em frequência de palavras até Redes Neurais Recorrentes e arquiteturas baseadas em Transformers.

A tarefa consiste em classificar textos financeiros escritos em português brasileiro em três classes:

* `negative`
* `neutral`
* `positive`

O foco do projeto não será apenas obter a maior acurácia, mas compreender o **trade-off entre desempenho, custo computacional, interpretabilidade, robustez, tempo de treinamento e qualidade das representações textuais aprendidas**.

O benchmark deverá comparar as seguintes abordagens:

* Regressão Logística com TF-IDF
* Rede Neural Recorrente treinada do zero
* Extração de embeddings contextuais combinada com LightGBM
* Fine-Tuning completo de um Transformer
* Fine-Tuning com LoRA
* Classificação Aumentada por Recuperação

Além da tarefa de classificação, o projeto também explorará:

* Pré-processamento de textos
* Tokenização
* Construção de vocabulário
* Representações esparsas com TF-IDF
* Embeddings treinados do zero
* Embeddings contextuais pré-treinados
* Redes Neurais Recorrentes
* LSTM e BiLSTM
* Transfer Learning
* Fine-Tuning
* LoRA
* Recuperação semântica
* Técnicas de interpretabilidade
* Análise de erros
* Otimização de hiperparâmetros
* Engenharia de atributos baseada em textos e embeddings

O notebook deverá servir tanto como um benchmark quanto como um material completo de estudo em Machine Learning, Deep Learning e Transformers aplicados ao Processamento de Linguagem Natural.

---

# Estrutura do Notebook

O notebook deverá ser desenvolvido integralmente em um único arquivo **Jupyter Notebook (`.ipynb`)**.

Nome esperado:

```text
benchmark_nlp_sentimento_financeiro.ipynb
```

A estrutura deverá seguir exatamente o mesmo padrão visual utilizado no notebook de Forecasting, utilizando títulos HTML semelhantes aos exemplos abaixo.

```html
# <font color='red' style='font-size: 40px;'> Título Principal </font>
<hr style='border: 2px solid red;'>
```

e

```html
# <font color='green' style='font-size: 30px;'> Subtítulo </font>
<hr style='border: 2px solid green;'>
```

Cada etapa do projeto deverá possuir sua própria seção contendo:

* Explicação teórica
* Fundamentação matemática, quando pertinente
* Código comentado
* Interpretação dos resultados
* Conclusões da etapa

Todo o notebook deverá ser escrito de forma didática, funcionando como um guia completo de estudo.

---

# Estrutura do Projeto

# <font color='red' style='font-size: 40px;'> Problema de Negócio </font>

* Objetivo do projeto
* Dataset utilizado
* Tipo de problema
* Variável-alvo
* Classes existentes
* Motivação do benchmark
* Aplicações da análise de sentimento financeiro
* Critérios de comparação entre os modelos

Possíveis aplicações:

* Classificação automática de notícias financeiras
* Monitoramento de comunicados corporativos
* Construção de indicadores de sentimento
* Identificação de notícias potencialmente negativas
* Priorização de textos para análise humana
* Criação de variáveis para modelos financeiros secundários

---

# <font color='red' style='font-size: 40px;'> Descrição do Dataset </font>

O dataset utilizado será o **Financial PhraseBank Portuguese Translation**.

Fonte:

```text
https://www.kaggle.com/datasets/mateuspicanco/financial-phrase-bank-portuguese-translation
```

Serão utilizadas exclusivamente as seguintes variáveis:

| Variável  | Descrição                                            |
| --------- | ---------------------------------------------------- |
| `text_pt` | Texto financeiro traduzido para português brasileiro |
| `y`       | Sentimento atribuído pelo consenso dos anotadores    |

A variável-alvo possui três classes:

* `negative`
* `neutral`
* `positive`

Ignorar completamente a coluna em inglês.

Analisar:

* Quantidade de observações
* Quantidade de textos únicos
* Distribuição das classes
* Valores ausentes
* Textos vazios
* Duplicidades
* Textos iguais com rótulos diferentes
* Possíveis textos ambíguos

---

# <font color='red' style='font-size: 40px;'> Hipóteses do Benchmark </font>

O projeto deverá avaliar as seguintes hipóteses:

1. TF-IDF combinado com Regressão Logística pode ser competitivo em um dataset pequeno e composto por textos curtos.

2. Uma Rede Neural Recorrente treinada do zero pode aprender relações sequenciais, mas pode apresentar limitações devido à quantidade reduzida de dados.

3. Embeddings contextuais pré-treinados podem produzir boas representações sem a necessidade de treinar um Transformer completo.

4. Embeddings contextuais combinados com LightGBM podem apresentar um bom equilíbrio entre desempenho e custo computacional.

5. O Fine-Tuning completo de um Transformer pode produzir o melhor desempenho, mas com maior custo computacional.

6. LoRA pode alcançar desempenho semelhante ao Fine-Tuning completo treinando apenas uma pequena parcela dos parâmetros.

7. A recuperação de exemplos semanticamente semelhantes pode melhorar a classificação de textos ambíguos.

8. O modelo mais complexo não necessariamente apresentará o melhor custo-benefício.

---

# <font color='red' style='font-size: 40px;'> Bibliotecas Utilizadas </font>

Importar todas as bibliotecas necessárias.

Exemplos:

* `pandas`
* `numpy`
* `matplotlib`
* `scikit-learn`
* `torch`
* `transformers`
* `datasets`
* `evaluate`
* `accelerate`
* `peft`
* `sentence-transformers`
* `lightgbm`
* `optuna`
* `captum`
* `shap`
* `tqdm`
* `joblib`
* `time`
* `os`
* `random`

Explicar resumidamente a finalidade de cada grupo de bibliotecas.

Definir:

```python
SEED = 42
```

Criar uma função para fixar as sementes em:

* `random`
* `numpy`
* `torch`
* `torch.cuda`

---

# <font color='red' style='font-size: 40px;'> Configuração do Ambiente </font>

Configurar:

* Seed global
* Dispositivo de processamento
* CPU ou GPU
* Diretório dos dados
* Diretório dos modelos
* Diretório dos checkpoints
* Diretório dos embeddings
* Diretório dos resultados

Exibir:

* Versão do Python
* Versão do PyTorch
* Versão do Transformers
* Disponibilidade da GPU
* Nome da GPU
* Memória disponível

---

# <font color='red' style='font-size: 40px;'> Funções Auxiliares </font>

Criar funções reutilizáveis para todo o projeto.

Exemplos:

* definição de seed
* limpeza de textos
* tokenização
* construção do vocabulário
* conversão de tokens para índices
* padding das sequências
* criação de Dataset
* criação de DataLoader
* cálculo de métricas
* matriz de confusão
* curvas ROC
* curvas Precision-Recall
* treinamento da RNN
* validação da RNN
* predição
* extração de embeddings
* medição de tempo
* contagem de parâmetros
* cálculo do tamanho do modelo
* comparação de modelos
* análise de erros
* armazenamento dos resultados

Criar, no mínimo:

```python
set_seed()
calcular_metricas()
plot_matriz_confusao()
plot_curvas_roc()
treinar_modelo_recorrente()
avaliar_modelo()
extrair_embeddings()
contar_parametros()
tamanho_modelo_mb()
medir_tempo_inferencia()
comparar_modelos()
```

Todas as funções deverão possuir docstrings e ser reutilizadas ao longo do notebook.

---

# <font color='red' style='font-size: 40px;'> Leitura dos Dados </font>

## Estrutura das Pastas

Os dados estarão disponíveis em:

```text
data/
```

## Carregamento do Dataset

Carregar exclusivamente:

```text
text_pt
y
```

## Validação dos Dados

Verificar:

* Primeiras observações
* Dimensões
* Tipos das variáveis
* Valores ausentes
* Textos vazios
* Textos compostos apenas por espaços
* Duplicidades
* Textos iguais com rótulos diferentes
* Distribuição da variável-alvo

## Codificação da Variável-Alvo

Utilizar o seguinte mapeamento:

```python
label2id = {
    "negative": 0,
    "neutral": 1,
    "positive": 2
}

id2label = {
    0: "negative",
    1: "neutral",
    2: "positive"
}
```

Manter o mesmo mapeamento em todos os experimentos.

---

# <font color='red' style='font-size: 40px;'> Separação dos Dados </font>

Separar os dados em:

* Treino
* Validação
* Teste

Utilizar divisão estratificada.

Proporção sugerida:

* 70% treino
* 15% validação
* 15% teste

A separação deverá ser realizada apenas uma vez.

Todos os modelos deverão utilizar exatamente os mesmos índices.

Garantir que:

* O conjunto de teste não seja utilizado no treinamento
* O teste não seja utilizado na otimização
* O teste não seja utilizado no early stopping
* Textos duplicados não apareçam em conjuntos diferentes
* A distribuição das classes seja preservada

Salvar os índices dos conjuntos.

---

# <font color='red' style='font-size: 40px;'> Análise Exploratória dos Textos </font>

Realizar uma EDA completa.

Exemplos:

* quantidade de textos por classe
* distribuição percentual das classes
* quantidade de caracteres
* quantidade de palavras
* quantidade de tokens
* comprimento médio por classe
* distribuição do comprimento dos textos
* textos mais curtos
* textos mais longos
* palavras mais frequentes
* bigramas mais frequentes
* termos financeiros mais recorrentes
* presença de negações
* presença de números
* presença de percentuais
* presença de símbolos monetários
* análise de possíveis problemas no dataset

Gerar gráficos com:

* título
* labels dos eixos
* legenda, quando aplicável

---

# <font color='red' style='font-size: 40px;'> Pré-processamento de Texto </font>

Explicar que cada metodologia deverá possuir seu próprio pipeline de preparação.

Não aplicar a mesma limpeza a todos os modelos.

---

## Texto Original

Preservar a coluna `text_pt` sem alterações para:

* Transformers
* Extração de embeddings
* LoRA
* Recuperação semântica

---

## Limpeza Básica

Criar uma versão moderadamente limpa para modelos clássicos e RNN.

Possíveis operações:

* remoção de espaços duplicados
* remoção de espaços no início e final
* padronização de quebras de linha
* correção de caracteres malformados
* tratamento de URLs
* tratamento de e-mails

Preservar:

* números
* percentuais
* sinais positivos e negativos
* símbolos monetários
* palavras de negação

Exemplos de informações relevantes:

```text
10%
R$ 5 milhões
US$ 2 bilhões
+15%
-8%
```

---

## Conversão para Minúsculas

Explicar:

* redução do vocabulário
* agrupamento de variações
* perda de informação de capitalização
* incompatibilidade com modelos `cased`

Aplicar lowercase somente no pipeline de TF-IDF ou RNN, caso o experimento justifique.

---

## Stopwords

Avaliar cuidadosamente a remoção de stopwords.

Nunca remover automaticamente:

* não
* sem
* mas
* apesar
* porém

Esses termos podem alterar completamente o sentimento da frase.

---

## Tratamento de Números

Preservar inicialmente os números.

Opcionalmente, comparar com tokens especiais:

```text
<PERCENTUAL>
<VALOR_MONETARIO>
<NUMERO>
```

Não substituir números sem avaliar o efeito sobre o desempenho.

---

## Tokenização

Explicar as diferenças entre:

* tokenização por palavras
* tokenização por regras
* tokenização por subpalavras
* WordPiece
* Byte-Pair Encoding
* SentencePiece

Mostrar exemplos de tokenização do mesmo texto em diferentes métodos.

---

# <font color='red' style='font-size: 40px;'> Baseline — Regressão Logística com TF-IDF </font>

## Representação TF-IDF

Explicar:

* matriz documento-termo
* frequência dos termos
* frequência inversa nos documentos
* esparsidade
* ausência de contexto semântico
* ausência de ordem global das palavras

Formalmente:

$$
TFIDF(t,d)=TF(t,d)\cdot IDF(t)
$$

Onde:

$$
TF(t,d)=
\frac{\text{frequência de }t\text{ no documento }d}
{\text{quantidade total de termos em }d}
$$

e:

$$
IDF(t)=
\log\left(
\frac{N}{DF(t)+1}
\right)
$$

---

## N-grams

Comparar:

* unigramas
* bigramas

Exemplo:

```text
não apresentou lucro
```

Unigramas:

```text
não
apresentou
lucro
```

Bigramas:

```text
não apresentou
apresentou lucro
```

Explicar como os bigramas preservam parcialmente expressões locais.

---

## Construção do Pipeline

Criar pipeline:

```text
Texto
↓
TF-IDF
↓
Regressão Logística Multinomial
↓
Probabilidades
↓
Classe
```

---

## Hiperparâmetros

Explorar moderadamente:

* `ngram_range`
* `min_df`
* `max_df`
* `max_features`
* `sublinear_tf`
* `C`
* `class_weight`

Não realizar uma busca excessivamente ampla.

---

## Avaliação

Apresentar:

* Accuracy
* Precision macro
* Recall macro
* F1-score macro
* F1-score por classe
* AUC multiclasse
* Log Loss
* Matriz de confusão
* Curvas ROC

---

## Interpretabilidade

Analisar:

* coeficientes mais positivos
* coeficientes mais negativos
* termos mais importantes por classe
* bigramas mais importantes

---

# <font color='red' style='font-size: 40px;'> Rede Neural Recorrente Desenvolvida do Zero </font>

Construir uma arquitetura recorrente completa em PyTorch.

A arquitetura principal poderá utilizar:

* RNN simples
* LSTM
* BiLSTM

A escolha final deverá ser justificada.

A rede deverá ser treinada inteiramente do zero, incluindo a camada de embeddings.

---

## Construção do Vocabulário

Explicar:

* frequência mínima
* tamanho do vocabulário
* palavras conhecidas
* palavras desconhecidas
* token de padding
* token desconhecido

Utilizar:

```python
special_tokens = {
    "<PAD>": 0,
    "<UNK>": 1
}
```

---

## Conversão para Índices

Exemplo:

```text
A empresa aumentou o lucro

↓

[18, 243, 781, 12, 965]
```

---

## Padding e Truncamento

Definir `max_length` a partir da distribuição real do comprimento dos textos.

Utilizar preferencialmente padding à direita.

Explicar:

* motivo do padding
* truncamento
* máscara
* efeito dos tokens artificiais

---

## Dataset e DataLoader

Criar classes personalizadas para armazenar:

* sequências
* comprimentos
* máscaras
* rótulos

Criar DataLoaders para:

* treino
* validação
* teste

---

## Camada de Embedding

A camada de embedding deverá ser inicializada aleatoriamente e aprendida durante o treinamento.

$$
\mathbf{e}_t=E[x_t]
$$

Onde:

* $x_t$ representa o índice do token
* $E$ representa a matriz de embeddings
* $\mathbf{e}_t$ representa o vetor denso do token

Explicar:

* tamanho do vocabulário
* dimensão do embedding
* embeddings treináveis
* diferença entre índice e vetor semântico

---

## Arquitetura Recorrente

Explicar:

* processamento sequencial
* estado oculto
* dependências temporais
* memória
* limitações da RNN simples
* vantagens da LSTM
* vantagens da bidirecionalidade

Para uma RNN simples:

$$
\mathbf{h}*t=
\tanh
\left(
W*{xh}\mathbf{x}*t+
W*{hh}\mathbf{h}_{t-1}+
\mathbf{b}_h
\right)
$$

---

## LSTM

Explicar:

* Forget Gate
* Input Gate
* Candidate State
* Cell State
* Output Gate

Destacar que a LSTM deverá ser utilizada caso apresente maior estabilidade que a RNN simples.

---

## BiLSTM

Explicar:

* processamento da esquerda para a direita
* processamento da direita para a esquerda
* concatenação dos estados

$$
\mathbf{h}_t=
[
\overrightarrow{\mathbf{h}}_t;
\overleftarrow{\mathbf{h}}_t
]
$$

---

## Pooling Temporal

Comparar:

* último estado oculto
* mean pooling
* max pooling
* mean-max pooling

Justificar a estratégia escolhida.

---

## Dropout

Explicar:

* regularização
* desligamento aleatório
* redução do overfitting

---

## Camada de Saída

A camada final deverá produzir três logits:

```text
negative
neutral
positive
```

Não aplicar Softmax dentro do `forward` quando utilizar:

```python
nn.CrossEntropyLoss()
```

Retornar:

```python
return logits
```

Durante a inferência:

```python
probabilidades = torch.softmax(logits, dim=1)
```

---

## Contagem de Parâmetros

Apresentar:

* parâmetros totais
* parâmetros treináveis
* tamanho do modelo
* formato das ativações
* dimensão dos embeddings
* dimensão do estado oculto

---

## Função de Perda

Utilizar:

```python
nn.CrossEntropyLoss()
```

Avaliar o uso de pesos de classe devido ao desbalanceamento.

---

## Otimizador

Comparar conceitualmente:

* SGD
* Adam
* AdamW

Justificar a escolha.

---

## Learning Rate

Explicar:

* influência no treinamento
* convergência
* instabilidade
* escolha inicial

---

## Gradient Clipping

Implementar gradient clipping para evitar explosão de gradientes.

Exemplo:

```python
torch.nn.utils.clip_grad_norm_(
    model.parameters(),
    max_norm
)
```

---

## Scheduler

Comparar:

* StepLR
* ReduceLROnPlateau
* Cosine Annealing

Justificar a escolha.

---

## Early Stopping

Utilizar F1-score macro de validação ou loss de validação.

Salvar o melhor checkpoint.

---

## Loop de Treinamento

Explicar:

* Forward Pass
* Cálculo da Loss
* Backpropagation
* Gradient Clipping
* Atualização dos Pesos
* Zero Grad
* Validação
* Salvamento do melhor modelo

---

## Acompanhamento do Treinamento

Gerar gráficos contendo:

* Loss de treino
* Loss de validação
* F1 macro de treino
* F1 macro de validação
* Learning Rate
* Tempo por época

Interpretar:

* overfitting
* underfitting
* convergência
* instabilidade

---

## Avaliação

Apresentar:

* Accuracy
* Precision macro
* Recall macro
* F1-score macro
* F1-score por classe
* AUC
* Log Loss
* Matriz de confusão
* Curvas ROC

---

## Otimização de Hiperparâmetros com Optuna

Otimizar somente a Rede Neural Recorrente.

Explicar:

* parâmetros versus hiperparâmetros
* Random Search
* Bayesian Optimization
* TPE
* Pruning

Definir espaço de busca para:

* dimensão do embedding
* dimensão do estado oculto
* número de camadas
* dropout
* learning rate
* weight decay
* batch size
* gradient clipping
* bidirecionalidade
* pooling temporal

Espaço inicial:

| Hiperparâmetro      | Valores                   |
| ------------------- | ------------------------- |
| Embedding dimension | 64, 128, 256              |
| Hidden dimension    | 64, 128, 256              |
| Número de camadas   | 1 a 3                     |
| Dropout             | 0,10 a 0,50               |
| Learning rate       | `1e-4` a `3e-3`           |
| Weight decay        | `1e-6` a `1e-3`           |
| Batch size          | 16, 32, 64                |
| Gradient clipping   | 0,5 a 5,0                 |
| Bidirecional        | True ou False             |
| Pooling             | last, mean, max, mean-max |

Métrica objetivo:

```text
F1-score macro de validação
```

Apresentar:

* melhores hiperparâmetros
* importância dos hiperparâmetros
* evolução dos trials
* melhor trial

---

## Avaliação Final

Comparar:

* RNN Base
* RNN Otimizada

Apresentar todas as métricas.

---

# <font color='red' style='font-size: 40px;'> Extração de Embeddings Contextuais </font>

Utilizar um modelo pré-treinado compatível com português.

Exemplos:

* Sentence Transformer multilíngue
* Encoder baseado em BERT compatível com português
* BERTimbau com estratégia de pooling

Explicar:

* embedding contextual
* representação vetorial da sentença
* dimensão do embedding
* diferença para TF-IDF
* diferença para embedding treinado do zero

---

## Estratégias de Pooling

Comparar:

* CLS Token
* Mean Pooling
* Max Pooling

Para Mean Pooling:

$$
\mathbf{e}=
\frac{
\sum_{t=1}^{T}m_t\mathbf{h}*t
}{
\sum*{t=1}^{T}m_t
}
$$

Onde:

* $\mathbf{h}_t$ representa o estado contextual do token
* $m_t$ representa a máscara de atenção
* $\mathbf{e}$ representa o embedding da sentença

---

## Extração dos Embeddings

Extrair embeddings para:

* treino
* validação
* teste

Salvar em:

* `.npy`
* `.parquet`

Garantir que nenhuma informação dos rótulos seja utilizada durante a extração.

---

# <font color='red' style='font-size: 40px;'> Classificação utilizando LightGBM </font>

Treinar um LightGBM utilizando os embeddings contextuais como features.

Fluxo:

```text
Texto
↓
Transformer pré-treinado
↓
Embedding contextual
↓
LightGBM
↓
Classe de sentimento
```

---

## Hiperparâmetros

Explorar moderadamente:

* `num_leaves`
* `max_depth`
* `learning_rate`
* `n_estimators`
* `min_child_samples`
* `subsample`
* `colsample_bytree`
* `reg_alpha`
* `reg_lambda`

Evitar uma otimização excessivamente ampla.

---

## Avaliação

Apresentar:

* Accuracy
* Precision macro
* Recall macro
* F1-score macro
* F1-score por classe
* AUC
* Log Loss
* Matriz de confusão

---

## Interpretabilidade

Utilizar:

* Feature Importance
* SHAP, quando viável

Destacar que cada dimensão do embedding não possui necessariamente uma interpretação semântica direta.

---

# <font color='red' style='font-size: 40px;'> Fine-Tuning de um Transformer </font>

Selecionar um Transformer pré-treinado para português.

Modelo principal sugerido:

```text
neuralmind/bert-base-portuguese-cased
```

Utilizar o mesmo Transformer nas etapas de Fine-Tuning completo e LoRA.

---

## Tokenização

Explicar:

* tokens especiais
* subpalavras
* `input_ids`
* `attention_mask`
* padding
* truncamento
* comprimento máximo

Mostrar exemplos reais da tokenização.

---

## Arquitetura do Transformer

Explicar:

* Token Embeddings
* Positional Embeddings
* Self-Attention
* Query
* Key
* Value
* Multi-Head Attention
* Feed-Forward Network
* Layer Normalization
* Conexões residuais
* Token de classificação

Self-Attention:

$$
Attention(Q,K,V)=
softmax
\left(
\frac{QK^T}{\sqrt{d_k}}
\right)V
$$

---

## Adaptação da Camada de Saída

Adaptar a cabeça de classificação para três classes.

Fluxo:

```text
Texto
↓
Tokenização
↓
Transformer Encoder
↓
Representação contextual
↓
Cabeça de classificação
↓
Três logits
```

---

## Fine-Tuning Completo

Descongelar todos os parâmetros do Transformer.

Configuração inicial sugerida:

* Learning rate: `2e-5` ou `5e-5`
* Batch size: 8, 16 ou 32
* Épocas: 3 a 5
* Weight decay: 0,01
* Warmup ratio: 0,10

Utilizar:

* AdamW
* Scheduler
* Warmup
* Gradient clipping
* Gradient accumulation
* Mixed precision
* Early stopping
* Melhor checkpoint por F1 macro

---

## Acompanhamento do Treinamento

Gerar gráficos de:

* Loss de treino
* Loss de validação
* F1 macro
* Learning rate
* Tempo por época

---

## Avaliação

Apresentar todas as métricas.

---

# <font color='red' style='font-size: 40px;'> Fine-Tuning utilizando LoRA </font>

Aplicar LoRA ao mesmo Transformer utilizado no Fine-Tuning completo.

Explicar:

* Low-Rank Adaptation
* Matrizes A e B
* Rank
* Alpha
* Dropout
* Módulos-alvo
* Congelamento dos pesos originais
* Parâmetros treináveis

A atualização será:

$$
W'=W+\Delta W
$$

com:

$$
\Delta W=BA
$$

Onde:

* $W$ representa a matriz original congelada
* $A$ e $B$ representam matrizes de baixa dimensão
* Apenas as matrizes de adaptação são treinadas

---

## Configuração Inicial

| Hiperparâmetro | Valores         |
| -------------- | --------------- |
| Rank `r`       | 4, 8, 16        |
| Alpha          | 8, 16, 32       |
| Dropout        | 0,05 a 0,20     |
| Learning rate  | `1e-5` a `5e-4` |
| Target Modules | Query e Value   |

---

## Comparação

Comparar:

* Fine-Tuning completo
* LoRA

Avaliar:

* F1 macro
* tempo de treinamento
* tempo por época
* memória utilizada
* parâmetros totais
* parâmetros treináveis
* percentual de parâmetros treináveis
* tamanho do checkpoint

---

# <font color='red' style='font-size: 40px;'> Classificação Aumentada por Recuperação </font>

Implementar uma abordagem de classificação apoiada por recuperação semântica.

Essa etapa não deverá ser tratada como um RAG generativo tradicional.

A ideia será recuperar textos rotulados semanticamente semelhantes e utilizar essas informações para melhorar a classificação.

---

## Construção do Índice Vetorial

Utilizar apenas textos do conjunto de treino.

Fluxo:

```text
Textos de treino
↓
Embeddings
↓
Índice vetorial
```

O índice poderá utilizar:

* busca exata por similaridade
* FAISS
* outro mecanismo vetorial simples

---

## Recuperação

Para cada texto de validação ou teste:

```text
Texto novo
↓
Embedding
↓
Busca semântica
↓
K textos mais similares do treino
```

---

## Distribuição dos Rótulos Recuperados

Calcular uma distribuição baseada nos vizinhos.

Exemplo:

$$
P_{retrieval}(y=c|x)
====================

\frac{
\sum_{i \in N_k(x)}
w_i\mathbb{1}(y_i=c)
}{
\sum_{i \in N_k(x)}w_i
}
$$

Onde:

* $N_k(x)$ representa os `K` vizinhos
* $w_i$ representa a similaridade
* $y_i$ representa o rótulo do vizinho

---

## Combinação com o Classificador

Combinar a probabilidade do modelo com a distribuição recuperada:

$$
P_{final}(y|x)
==============

\lambda P_{modelo}(y|x)
+
(1-\lambda)P_{retrieval}(y|x)
$$

Onde:

* $P_{modelo}$ representa a probabilidade do classificador-base
* $P_{retrieval}$ representa a distribuição dos vizinhos
* $\lambda$ controla a combinação

O classificador-base poderá ser:

* Regressão Logística
* LightGBM
* Transformer

Selecionar `K` e $\lambda$ utilizando apenas a validação.

---

## Retrieval-Augmented Few-Shot — Opcional

Opcionalmente, utilizar os textos recuperados como exemplos em um prompt de classificação.

Fluxo:

```text
Texto semelhante 1 + rótulo
Texto semelhante 2 + rótulo
Texto semelhante 3 + rótulo
Texto novo
↓
Modelo de linguagem
↓
Classe prevista
```

Essa etapa deverá permanecer opcional.

---

## Cuidados contra Leakage

* Indexar apenas textos de treino
* Nunca indexar validação ou teste
* Remover duplicidades
* Não utilizar o rótulo da consulta
* Selecionar hiperparâmetros apenas na validação
* Registrar os textos recuperados
* Verificar textos quase idênticos

---

## Avaliação

Comparar:

* classificador sem recuperação
* classificador com recuperação

Responder:

* a recuperação melhorou o F1 macro?
* quais classes foram mais beneficiadas?
* textos ambíguos foram melhor classificados?
* o ganho justificou o aumento de latência?

---

# <font color='red' style='font-size: 40px;'> Interpretabilidade </font>

Aplicar técnicas compatíveis com cada metodologia.

---

## Regressão Logística

Analisar:

* coeficientes
* palavras importantes
* bigramas importantes
* contribuições positivas e negativas

---

## Rede Neural Recorrente

Utilizar:

* Integrated Gradients
* importância dos tokens
* gradientes sobre embeddings

---

## LightGBM

Utilizar:

* Feature Importance
* SHAP

---

## Transformer

Utilizar:

* Token Attribution
* Integrated Gradients
* Attention Maps como análise complementar

Destacar que pesos de atenção não representam necessariamente uma explicação causal completa.

---

## Recuperação Semântica

Mostrar:

* texto consultado
* textos recuperados
* similaridades
* rótulos dos vizinhos
* distribuição final dos rótulos

---

# <font color='red' style='font-size: 40px;'> Robustez dos Modelos </font>

Criar versões perturbadas dos textos de teste.

Exemplos:

* remoção de pontuação
* alteração de capitalização
* remoção de acentos
* espaços duplicados
* erros ortográficos leves
* substituição controlada de sinônimos

As perturbações não podem alterar o sentimento real.

Avaliar:

* queda de Accuracy
* queda de F1 macro
* mudança de classe
* variação das probabilidades

---

# <font color='red' style='font-size: 40px;'> Análise de Erros </font>

Analisar erros relacionados a:

* negação
* contraste entre orações
* sentimento implícito
* comparação com expectativas
* números
* percentuais
* termos financeiros
* textos ambíguos
* textos muito curtos
* textos muito longos
* confusão entre neutro e positivo
* confusão entre neutro e negativo

Criar tabela contendo:

| Texto | Classe real | Classe prevista | Probabilidades | Modelo | Categoria do erro |
| ----- | ----------- | --------------- | -------------- | ------ | ----------------- |

Selecionar exemplos:

* acertados por todos
* errados por todos
* acertados apenas pelo Transformer
* acertados apenas pelo TF-IDF
* corrigidos pela recuperação
* prejudicados pela recuperação

---

# <font color='red' style='font-size: 40px;'> Comparação dos Modelos </font>

Construir uma tabela consolidada contendo:

* Modelo
* Representação textual
* Accuracy
* Precision macro
* Recall macro
* F1-score macro
* F1-score por classe
* AUC
* Log Loss
* Tempo de treinamento
* Tempo de inferência
* Latência por observação
* Número de parâmetros
* Número de parâmetros treináveis
* Percentual de parâmetros treináveis
* Uso aproximado de memória
* Tempo por época
* Tamanho do modelo salvo

Modelos obrigatórios:

* Regressão Logística + TF-IDF
* RNN Base
* RNN Otimizada
* Embeddings + LightGBM
* Transformer com Fine-Tuning completo
* Transformer com LoRA
* Classificação Aumentada por Recuperação

Gerar gráficos comparativos.

---

# <font color='red' style='font-size: 40px;'> Trade-offs </font>

Discutir:

* desempenho
* custo computacional
* velocidade de treinamento
* velocidade de inferência
* necessidade de GPU
* quantidade de parâmetros
* interpretabilidade
* robustez
* escalabilidade
* facilidade de implementação
* facilidade de manutenção
* capacidade de generalização

Responder:

* Quando TF-IDF é suficiente?
* Quando vale a pena treinar uma RNN do zero?
* Quando embeddings + LightGBM são suficientes?
* Quando utilizar Fine-Tuning completo?
* Quando utilizar LoRA?
* Quando a recuperação semântica agrega valor?
* Qual abordagem apresenta o melhor custo-benefício?

---

# <font color='red' style='font-size: 40px;'> Engenharia de Atributos baseada em Textos e Embeddings </font>

Construir indicadores derivados dos textos.

---

## Indicadores Lexicais

* quantidade de caracteres
* quantidade de palavras
* comprimento médio das palavras
* quantidade de números
* quantidade de percentuais
* quantidade de símbolos monetários
* presença de negação
* quantidade de palavras em caixa alta

---

## Indicadores baseados em Probabilidades

* probabilidade da classe positiva
* probabilidade da classe neutra
* probabilidade da classe negativa
* confiança máxima
* entropia
* margem entre primeira e segunda classe

Entropia:

$$
H(\mathbf{p})
=============

-\sum_{c=1}^{C}
p_c\log(p_c)
$$

Margem:

$$
M=p_{(1)}-p_{(2)}
$$

---

## Indicadores baseados em Embeddings

* norma L2
* similaridade com textos positivos
* similaridade com textos neutros
* similaridade com textos negativos
* distância ao centroide de cada classe
* distância ao vizinho mais próximo
* similaridade média dos vizinhos recuperados
* concordância entre o modelo e a recuperação

---

## Divergência entre Modelos

Criar indicadores como:

* TF-IDF e Transformer discordam
* RNN e Transformer discordam
* LightGBM e LoRA discordam
* classificador-base e recuperação discordam

Esses indicadores poderão ser utilizados para:

* revisão humana
* priorização
* detecção de incerteza
* monitoramento
* modelos secundários

---

# <font color='red' style='font-size: 40px;'> Conclusões </font>

Responder, entre outras, às seguintes perguntas:

* Qual estratégia apresentou a melhor performance?
* Qual apresentou o melhor custo-benefício?
* TF-IDF foi competitivo?
* A RNN treinada do zero foi capaz de aprender com poucos dados?
* Quanto a otimização melhorou a RNN?
* Embeddings + LightGBM foram competitivos?
* Fine-Tuning completo justificou o custo?
* LoRA alcançou desempenho próximo ao Fine-Tuning completo?
* A recuperação semântica trouxe ganho?
* Quais classes foram mais difíceis?
* Quais limitações foram encontradas?
* Quais são os possíveis trabalhos futuros?

---

# Requisitos Gerais

Durante todo o desenvolvimento:

* Utilizar apenas a coluna `text_pt`.
* Utilizar `y` como variável-alvo.
* Utilizar PyTorch para a Rede Neural Recorrente.
* Utilizar scikit-learn para TF-IDF e Regressão Logística.
* Utilizar LightGBM sobre embeddings.
* Utilizar Hugging Face Transformers.
* Utilizar PEFT para LoRA.
* Utilizar Optuna apenas para a RNN.
* Manter código limpo, modular e reutilizável.
* Comentar detalhadamente todos os blocos de código.
* Explicar matematicamente os principais conceitos.
* Interpretar todos os resultados.
* Encapsular trechos repetitivos em funções.
* Garantir reprodutibilidade utilizando seed.
* Utilizar F1-score macro como métrica principal.
* Utilizar os mesmos splits em todos os modelos.
* Não utilizar o teste para seleção de modelos.
* Evitar data leakage.
* Salvar os melhores checkpoints.
* Salvar os embeddings.
* Registrar hiperparâmetros.
* Registrar tempo de execução.
* Registrar consumo aproximado de memória.
* Não inventar métricas.
* Comparar todas as abordagens sob os mesmos critérios.
* Justificar tecnicamente todas as decisões.

---

# Ordem Recomendada dos Experimentos

```text
1. Leitura e validação dos dados

↓

2. Análise exploratória

↓

3. Regressão Logística + TF-IDF

↓

4. RNN Base

↓

5. Otimização da RNN com Optuna

↓

6. Extração de embeddings contextuais

↓

7. Embeddings + LightGBM

↓

8. Fine-Tuning completo do Transformer

↓

9. Fine-Tuning com LoRA

↓

10. Classificação Aumentada por Recuperação

↓

11. Interpretabilidade

↓

12. Robustez e análise de erros

↓

13. Comparação final

↓

14. Conclusões
```

---

# Exemplo de Organização de Código

Sempre deixar os códigos:

* comentados
* identados
* modulares
* didáticos
* separados por responsabilidade

Criar células Markdown antes dos principais blocos para explicar:

* o que será executado
* por que a operação é necessária
* qual é o formato da entrada
* qual é o formato da saída
* qual é a fundamentação matemática
* como interpretar o resultado

Exemplo:

```python
# =========================================================
# DEFINIÇÃO DA ARQUITETURA DA LSTM
# =========================================================

# Define uma nova classe chamada DSALSTM
# Toda rede neural criada no PyTorch deve herdar de nn.Module
class DSALSTM(nn.Module):

    # =====================================================
    # CONSTRUTOR DA REDE
    # Aqui definimos todas as camadas da arquitetura
    # =====================================================
    def __init__(
        self,
        vocab_size,
        embedding_dim,
        hidden_dim,
        output_dim,
        num_layers = 1,
        dropout = 0.30,
        padding_idx = 0,
        bidirectional = True
    ):

        # Inicializa a infraestrutura da classe nn.Module
        super().__init__()

        # =================================================
        # CAMADA DE EMBEDDING
        # =================================================

        # Converte índices inteiros em vetores densos
        #
        # Entrada:
        #   (batch_size, sequence_length)
        #
        # Saída:
        #   (batch_size, sequence_length, embedding_dim)
        self.embedding = nn.Embedding(
            num_embeddings = vocab_size,
            embedding_dim = embedding_dim,
            padding_idx = padding_idx
        )

        # =================================================
        # CAMADA LSTM
        # =================================================

        # Processa a sequência token por token
        #
        # bidirectional=True cria:
        # - uma LSTM da esquerda para a direita
        # - uma LSTM da direita para a esquerda
        self.lstm = nn.LSTM(
            input_size = embedding_dim,
            hidden_size = hidden_dim,
            num_layers = num_layers,
            dropout = dropout if num_layers > 1 else 0.0,
            batch_first = True,
            bidirectional = bidirectional
        )

        # =================================================
        # REGULARIZAÇÃO
        # =================================================

        self.dropout = nn.Dropout(p = dropout)

        # =================================================
        # CAMADA DE CLASSIFICAÇÃO
        # =================================================

        # Em uma BiLSTM, a dimensão é duplicada
        lstm_output_dim = (
            hidden_dim * 2
            if bidirectional
            else hidden_dim
        )

        # Produz três logits:
        # - negative
        # - neutral
        # - positive
        self.classifier = nn.Linear(
            in_features = lstm_output_dim,
            out_features = output_dim
        )

    # =====================================================
    # FORWARD PASS
    # =====================================================
    def forward(self, input_ids):

        # Converte os índices em embeddings
        embeddings = self.embedding(input_ids)

        # Processa toda a sequência
        outputs, (hidden, cell) = self.lstm(embeddings)

        # Caso a rede seja bidirecional:
        # concatena o estado final das duas direções
        if self.lstm.bidirectional:

            forward_hidden = hidden[-2]
            backward_hidden = hidden[-1]

            sentence_representation = torch.cat(
                (forward_hidden, backward_hidden),
                dim = 1
            )

        else:

            sentence_representation = hidden[-1]

        # Aplica regularização
        sentence_representation = self.dropout(
            sentence_representation
        )

        # Produz os logits
        logits = self.classifier(
            sentence_representation
        )

        # CrossEntropyLoss recebe logits diretamente
        return logits
```

---

# Observação sobre a Camada de Saída

Durante o treinamento:

```python
return logits
```

Não utilizar:

```python
return torch.softmax(logits, dim = 1)
```

quando a função de perda for:

```python
nn.CrossEntropyLoss()
```

Durante a inferência:

```python
probabilidades = torch.softmax(
    logits,
    dim = 1
)
```
