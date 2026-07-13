# Resumo do Paper — *Good Debt or Bad Debt: Detecting Semantic Orientations in Economic Texts*

---

## Referência

**Malo, P.; Sinha, A.; Takala, P.; Korhonen, P.; Wallenius, J.**  
*Good Debt or Bad Debt: Detecting Semantic Orientations in Economic Texts*.

**Tema:** análise de sentimento em textos financeiros e econômicos.  
**Tarefa:** classificação multiclasse de frases em `positive`, `neutral` ou `negative`.  
**Principal contribuição de dados:** criação do **Financial PhraseBank**.

---

## 1. Resumo Executivo

O paper investiga como identificar corretamente a **orientação semântica** de frases financeiras. O ponto de partida é que a polaridade de uma frase não pode ser determinada apenas pela contagem de palavras positivas e negativas.

No domínio financeiro, muitos conceitos são neutros quando observados isoladamente, mas passam a expressar sentimento quando combinados com verbos ou expressões que indicam direção. Por exemplo:

```text
O lucro foi de 10 milhões.
```

A frase apenas apresenta uma informação e tende a ser classificada como neutra.

```text
O lucro aumentou em relação ao ano anterior.
```

A combinação entre o conceito financeiro `lucro` e a direção `aumentou` produz orientação positiva.

```text
O prejuízo diminuiu no trimestre.
```

Embora `prejuízo` seja normalmente associado a algo desfavorável, sua redução representa um evento positivo para o investidor.

Para representar essas relações, os autores propõem o modelo **Linearized Phrase Structure — LPS**, que combina:

1. um léxico especializado no domínio financeiro;
2. identificação de entidades e influenciadores de polaridade;
3. informações sobre a estrutura da frase;
4. regras de combinação e poda de entidades;
5. um classificador SVM multiclasse.

Os autores também constroem um corpus com aproximadamente 5 mil frases financeiras anotadas por pessoas com formação em negócios. Esse corpus se tornou conhecido como **Financial PhraseBank** e é amplamente utilizado como benchmark de análise de sentimento financeiro.

---

## 2. Problema de Pesquisa

Métodos tradicionais de análise de sentimento costumam utilizar abordagens como:

- contagem de palavras positivas e negativas;
- Bag-of-Words;
- dicionários genéricos de polaridade;
- frequências de termos;
- classificadores treinados sobre representações esparsas.

Essas abordagens apresentam limitações importantes quando aplicadas ao domínio financeiro.

### 2.1. Dependência do domínio

Uma palavra considerada negativa em linguagem geral pode ser neutra ou possuir outro significado em textos financeiros. O artigo menciona como referência o trabalho de Loughran e McDonald, que mostrou que dicionários genéricos classificam incorretamente diversos termos presentes em documentos corporativos.

### 2.2. Diferença entre polaridade prévia e contextual

A **polaridade prévia** corresponde ao sentimento normalmente associado a uma palavra quando ela é observada isoladamente.

A **polaridade contextual** corresponde ao sentimento assumido pela expressão dentro de uma frase específica.

Assim, a presença de um termo positivo ou negativo não determina, sozinha, a orientação semântica da frase completa.

### 2.3. Importância da direção dos eventos

Em finanças, o sentimento frequentemente depende da interação entre:

```text
conceito financeiro + direção do evento
```

Exemplos:

| Conceito | Direção | Orientação provável |
|---|---|---|
| Lucro | Aumentar | Positiva |
| Lucro | Diminuir | Negativa |
| Prejuízo | Aumentar | Negativa |
| Prejuízo | Diminuir | Positiva |
| Dívida | Aumentar | Geralmente negativa |
| Receita | Aumentar | Geralmente positiva |

Essa característica motiva a incorporação explícita de conhecimento financeiro ao modelo.

---

## 3. Objetivos do Paper

O trabalho possui três objetivos principais.

### 3.1. Construir um corpus financeiro anotado

Criar um conjunto de frases provenientes de notícias e comunicados corporativos, classificadas por humanos como:

- positiva;
- neutra;
- negativa.

### 3.2. Enriquecer léxicos financeiros

Adicionar informações que permitam representar:

- conceitos financeiros;
- verbos de movimento;
- expressões direcionais;
- negações;
- intensificadores;
- modalizadores;
- dependência entre conceito e direção.

### 3.3. Desenvolver o modelo LPS

Construir uma representação linear da estrutura sintática das frases que preserve os elementos mais importantes para a classificação de sentimento, sem depender da explosão dimensional causada por grandes conjuntos de n-gramas.

---

## 4. Principais Contribuições

### 4.1. Financial PhraseBank

O paper apresenta um corpus de aproximadamente 5 mil frases financeiras anotadas por especialistas ou estudantes com formação adequada em finanças, economia e contabilidade.

Esse corpus permite:

- treinar modelos supervisionados;
- comparar algoritmos sob um benchmark comum;
- estudar divergências entre anotadores;
- avaliar a dificuldade de distinguir frases positivas, negativas e neutras.

### 4.2. Léxico financeiro sensível à direção

Os autores ampliam léxicos existentes por meio da inclusão de conceitos financeiros e expressões que indicam mudança.

Os conceitos são divididos, entre outros, em:

- **positive-if-up:** tornam-se positivos quando aumentam;
- **negative-if-up:** tornam-se negativos quando aumentam.

Exemplos:

```text
EBIT → positive-if-up
Liabilities → negative-if-up
```

### 4.3. Linearized Phrase Structure — LPS

O LPS representa a frase como uma sequência ordenada de entidades semânticas. Em vez de conservar todas as palavras e todas as combinações possíveis, o modelo mantém elementos considerados relevantes para a orientação financeira.

---

## 5. Construção do Financial PhraseBank

### 5.1. Origem dos textos

O corpus foi construído com notícias em inglês relacionadas às empresas listadas na bolsa de Helsinque, obtidas na base LexisNexis.

O procedimento geral foi:

```text
10.000 notícias selecionadas
        ↓
Extração das sentenças
        ↓
Retenção das frases com entidades do léxico
        ↓
53.400 frases candidatas
        ↓
Amostragem de aproximadamente 5.000 frases
        ↓
Anotação humana
```

A seleção procurou representar:

- empresas pequenas e grandes;
- diferentes setores econômicos;
- diferentes fontes de notícias;
- diferentes estruturas linguísticas.

### 5.2. Anotadores

Participaram **16 anotadores**:

- 3 pesquisadores;
- 13 estudantes de mestrado da Aalto University School of Business.

As principais áreas de formação eram:

- finanças;
- contabilidade;
- economia.

Cada frase recebeu entre **5 e 8 avaliações**.

### 5.3. Perspectiva de anotação

As frases foram classificadas do ponto de vista de um investidor. A pergunta implícita era:

> A informação apresentada tende a exercer efeito positivo, neutro ou negativo sobre a empresa e seu valor?

Os anotadores deveriam considerar apenas o conteúdo explícito da frase, evitando inferências baseadas em informações externas sobre a empresa.

### 5.4. Distribuição das classes

Os autores criaram quatro versões do corpus com base no grau de concordância entre os anotadores.

| Critério de concordância | Negativa | Neutra | Positiva | Frases |
|---|---:|---:|---:|---:|
| 100% | 13,4% | 61,4% | 25,2% | 2.259 |
| Maior que 75% | 12,2% | 62,1% | 25,7% | 3.448 |
| Maior que 66% | 12,2% | 60,1% | 27,7% | 4.211 |
| Maior que 50% | 12,5% | 59,4% | 28,2% | 4.840 |

A classe neutra é majoritária em todas as versões.

### 5.5. Concordância entre anotadores

A concordância média geral entre pares de anotadores foi de aproximadamente **74,9%**.

| Comparação | Concordância |
|---|---:|
| Positiva vs. negativa | 98,7% |
| Negativa vs. neutra | 94,2% |
| Positiva vs. neutra | 75,2% |

O principal ponto de dificuldade foi separar frases **positivas** de frases **neutras**. Comunicados corporativos frequentemente utilizam linguagem otimista ou promocional, mesmo quando a informação econômica concreta é limitada.

---

## 6. Léxico Financeiro

O léxico utilizado contém mais de **10 mil entradas** e combina recursos gerais e específicos do domínio financeiro.

| Categoria | Quantidade | Participação aproximada |
|---|---:|---:|
| Expressões positivas gerais | 2.933 | 27,8% |
| Expressões negativas gerais | 5.951 | 56,3% |
| Expressões neutras gerais | 585 | 5,5% |
| Conceitos `positive-if-up` | 252 | 2,4% |
| Conceitos `negative-if-up` | 95 | 0,9% |
| Direções de queda | 128 | 1,2% |
| Direções de alta | 186 | 1,8% |
| Reversores de polaridade | 188 | 1,8% |
| Modalizadores | 50 | 0,5% |
| Termos litigiosos | 95 | 0,9% |
| Termos de incerteza | 102 | 1,0% |

### 6.1. Expressões gerais de polaridade

Os autores partem do léxico MPQA e incorporam termos do léxico financeiro de Loughran e McDonald. Em situações de conflito, prevalece a classificação específica do domínio financeiro.

### 6.2. Entidades financeiras

Cada entidade financeira pode conter:

- conceito subjacente;
- expressão textual usada para identificá-lo;
- polaridade prévia;
- dependência direcional.

A maioria dos conceitos financeiros é tratada como neutra até que uma expressão de direção seja encontrada.

### 6.3. Influenciadores de polaridade

O modelo considera elementos capazes de modificar a orientação contextual:

- negações;
- intensificadores;
- redutores de intensidade;
- modalizadores;
- expressões de incerteza;
- verbos ou expressões de alta e queda.

---

## 7. Modelo Linearized Phrase Structure — LPS

O modelo é construído em três etapas principais:

```text
1. Extração de entidades semânticas
2. Projeção linear da estrutura da frase
3. Classificação multiclasse
```

### 7.1. Extração de entidades

A frase é inicialmente analisada sintaticamente. Em seguida, o algoritmo procura trechos correspondentes às entradas do léxico.

O resultado é uma sequência ordenada de entidades:

```text
e₁, e₂, ..., eₙ
```

Cada entidade representa uma unidade com informação relevante para a orientação semântica.

### 7.2. Regras de poda

Após a detecção, o modelo aplica regras para reduzir redundâncias e combinar elementos relacionados.

#### União de entidades neutras

Sequências consecutivas de elementos neutros podem ser agregadas em uma única entidade.

#### Combinação com influenciadores

Quando uma expressão direcional modifica um conceito financeiro, as duas entidades são combinadas.

Exemplo conceitual:

```text
Lucro + aumentou
        ↓
Positivo-if-up + direção de alta
        ↓
Orientação positiva
```

Outro exemplo:

```text
Erros de cobrança + diminuíram
        ↓
Negative-if-up + direção de queda
        ↓
Orientação positiva
```

### 7.3. Projeção da estrutura da frase

A sequência de entidades é convertida em uma representação binária. Cada tipo de entidade possui um código indicador.

Duas frases são consideradas estruturalmente equivalentes quando apresentam:

- o mesmo número de entidades;
- os mesmos tipos de entidade;
- a mesma ordem relativa.

Essa representação procura preservar a estrutura semântica importante sem gerar todos os n-gramas possíveis.

### 7.4. Classificador

Após a projeção, os autores utilizam uma **Support Vector Machine multiclasse**, com estratégia **one-vs-one**.

Para três classes, são treinados classificadores binários para os pares:

```text
positivo vs. negativo
positivo vs. neutro
negativo vs. neutro
```

A classe final é definida a partir da combinação das decisões dos classificadores.

---

## 8. Modelos Comparados

O paper compara cinco estratégias.

### 8.1. W-MPQA

Contagem de palavras positivas e negativas utilizando o léxico MPQA.

### 8.2. W-Loughran

Contagem de palavras utilizando o léxico financeiro de Loughran e McDonald.

### 8.3. SVM-MPQA

Modelo de sequência de polaridades treinado com SVM e léxico MPQA.

### 8.4. R-LPS

Versão reduzida do LPS sem as regras de poda e combinação de entidades.

### 8.5. LPS

Modelo completo, contendo:

- léxico financeiro enriquecido;
- entidades financeiras;
- expressões direcionais;
- influenciadores de polaridade;
- regras de poda;
- projeção da estrutura;
- SVM multiclasse.

---

## 9. Protocolo Experimental

Os modelos com etapa de aprendizado foram avaliados por meio de **validação cruzada com 10 folds**.

Os experimentos foram executados separadamente nas quatro versões do Financial PhraseBank, definidas pelo grau de concordância entre os anotadores.

As principais métricas foram:

- Accuracy;
- Precision;
- Recall;
- F1-score.

O artigo reporta resultados separadamente para cada classe, tratando cada uma como positiva contra as demais para o cálculo das métricas apresentadas.

---

## 10. Principais Resultados

Os modelos LPS e R-LPS superaram as abordagens baseadas apenas em contagem de palavras e o modelo SVM-MPQA na maioria das comparações.

### 10.1. Frases com 100% de concordância

#### Classe positiva

| Modelo | Accuracy | Recall | Precision | F1-score |
|---|---:|---:|---:|---:|
| W-MPQA | 0,659 | 0,519 | 0,374 | 0,435 |
| W-Loughran | 0,755 | 0,125 | 0,563 | 0,204 |
| SVM-MPQA | 0,746 | 0,060 | 0,466 | 0,106 |
| R-LPS | 0,858 | 0,698 | 0,728 | 0,713 |
| **LPS** | **0,869** | **0,737** | **0,742** | **0,739** |

#### Classe neutra

| Modelo | Accuracy | Recall | Precision | F1-score |
|---|---:|---:|---:|---:|
| W-MPQA | 0,586 | 0,581 | 0,694 | 0,632 |
| W-Loughran | 0,625 | 0,914 | 0,635 | 0,750 |
| SVM-MPQA | 0,652 | 0,963 | 0,645 | 0,773 |
| R-LPS | **0,851** | **0,887** | **0,872** | **0,880** |
| LPS | 0,828 | 0,868 | 0,854 | 0,861 |

#### Classe negativa

| Modelo | Accuracy | Recall | Precision | F1-score |
|---|---:|---:|---:|---:|
| W-MPQA | 0,829 | 0,370 | 0,364 | 0,367 |
| W-Loughran | 0,849 | 0,162 | 0,360 | 0,223 |
| SVM-MPQA | 0,870 | 0,205 | 0,534 | 0,296 |
| R-LPS | 0,947 | **0,799** | 0,801 | 0,800 |
| **LPS** | **0,951** | 0,789 | **0,839** | **0,813** |

### 10.2. Interpretação dos resultados

Os resultados sustentam três conclusões:

1. **Um léxico específico do domínio é superior a um léxico genérico.**
2. **Somente melhorar o léxico não é suficiente:** a forma de combinar os elementos da frase também importa.
3. **As regras de poda e interação agregam valor**, pois o LPS completo geralmente supera sua versão reduzida.

O ganho é especialmente expressivo no recall e no F1-score das classes positiva e negativa. Modelos simples frequentemente classificam a maior parte dos exemplos como neutra, obtendo accuracy aparentemente razoável, mas baixo recall nas classes minoritárias.

---

## 11. Análise de Erros

Os autores analisaram uma amostra aleatória de 300 frases classificadas incorretamente pelo LPS.

| Fonte de erro | Percentual dos erros | Como único erro |
|---|---:|---:|
| Necessidade de mais contexto | 43,0% | 20,0% |
| Incapacidade de reconhecer a relevância do evento | 26,7% | 19,3% |
| Linguagem publicitária da própria empresa | 10,0% | 3,3% |
| Correspondências incorretas no léxico | 9,0% | 7,7% |
| Polissemia | 8,7% | 0,7% |
| Convenção de descrever eventos positivamente | 6,3% | 2,0% |
| Expressões ausentes do léxico | 6,0% | 1,0% |
| Incapacidade de avaliar magnitude e valor | 5,3% | 0,3% |
| Incapacidade de detectar mudanças numéricas | 4,3% | 3,7% |
| Incapacidade de identificar papéis na frase | 4,3% | 0,3% |
| Erros nas regras de poda | 4,0% | 0,3% |
| Incapacidade de detectar expressões temporais | 3,3% | 2,7% |
| Casos ambíguos para humanos | 2,7% | 1,0% |

### 11.1. Falta de contexto

Uma frase isolada pode não fornecer informações suficientes para determinar seu impacto econômico. Aquisições, nomeações de executivos e parcerias podem ser apresentadas positivamente, embora seu efeito real dependa de informações externas.

### 11.2. Linguagem corporativa promocional

Comunicados produzidos pelas próprias empresas utilizam frequentemente linguagem otimista. O modelo pode interpretar esse tom como evidência objetiva de desempenho positivo.

### 11.3. Relevância e magnitude

O algoritmo tem dificuldade para avaliar se uma mudança é economicamente material. Um crescimento pequeno pode não justificar uma classificação positiva, mesmo que a frase contenha termos de alta.

### 11.4. Perspectiva

Uma mesma notícia pode ser positiva para uma empresa e negativa para outra. A identificação correta depende de compreender os papéis semânticos e a entidade sob análise.

---

## 12. Conclusões do Paper

O artigo conclui que a análise de sentimento financeiro exige uma combinação de:

- conhecimento específico do domínio;
- léxicos de alta qualidade;
- identificação de conceitos econômicos;
- tratamento de expressões direcionais;
- representação da estrutura contextual;
- algoritmos supervisionados capazes de aprender interações.

A simples contagem de palavras não representa adequadamente frases nas quais o sentimento depende da relação entre conceitos e eventos.

O modelo LPS demonstrou que uma representação linguística relativamente parcimoniosa pode superar abordagens baseadas exclusivamente em frequências, sem exigir a geração de um espaço muito grande de n-gramas.

---

## 13. Pontos Fortes

- Introduz um corpus financeiro anotado por pessoas com formação relevante.
- Formaliza a diferença entre polaridade lexical e orientação contextual.
- Representa explicitamente a direção de eventos financeiros.
- Compara o modelo com baselines de diferentes níveis de complexidade.
- Avalia o efeito do grau de concordância entre anotadores.
- Realiza análise qualitativa detalhada dos erros.
- Apresenta uma solução mais interpretável do que modelos neurais modernos.

---

## 14. Limitações

- O corpus contém frases isoladas, sem o contexto completo do documento.
- O modelo depende fortemente da cobertura e da qualidade do léxico.
- As regras linguísticas exigem manutenção manual.
- O sistema possui dificuldade com polissemia, magnitude, temporalidade e perspectiva.
- A validação cruzada não representa necessariamente um cenário temporal ou fora da amostra.
- Os parâmetros dos modelos foram utilizados sem otimização específica.
- A abordagem foi desenvolvida antes da popularização de embeddings contextuais e Transformers.
- Os resultados não devem ser comparados diretamente com benchmarks modernos sem reproduzir os mesmos splits e critérios de avaliação.

---

## 15. Relação com Modelos Modernos de NLP

O LPS pode ser interpretado como uma abordagem anterior aos Transformers que busca representar contexto por meio de regras explícitas.

| LPS | Transformer moderno |
|---|---|
| Entidades definidas por léxico | Representações aprendidas nos embeddings |
| Direcionalidade codificada por regras | Relações aprendidas por Self-Attention |
| Estrutura sintática processada explicitamente | Contexto capturado pelas camadas do encoder |
| SVM sobre representação linear | Cabeça neural de classificação |
| Elevada interpretabilidade | Representações mais complexas e menos transparentes |
| Forte dependência de engenharia manual | Forte dependência de pré-treinamento e dados |

Apesar da evolução dos modelos, várias ideias do paper continuam relevantes:

- necessidade de conhecimento específico do domínio;
- importância de negações e expressões direcionais;
- dificuldade da distinção entre positivo e neutro;
- risco de leakage entre frases repetidas;
- importância de avaliar F1 por classe e não apenas Accuracy;
- necessidade de analisar erros semanticamente.

---

## 16. Relevância para o Projeto de Benchmark em Português

O paper é a principal referência conceitual para um benchmark baseado na tradução portuguesa do Financial PhraseBank.

Ele justifica a comparação entre:

- Regressão Logística com TF-IDF;
- RNN treinada do zero;
- LightGBM sobre embeddings contextuais;
- Fine-Tuning de Transformer;
- LoRA;
- Classificação Aumentada por Recuperação.

A abordagem de recuperação semântica também pode responder a uma limitação identificada pelos autores: a falta de contexto. Embora recuperar frases semelhantes não forneça o documento original completo, os vizinhos podem oferecer evidências adicionais sobre como estruturas semanticamente próximas foram classificadas no conjunto de treinamento.

### Hipóteses para o benchmark

1. **TF-IDF pode ser competitivo**, pois as frases são curtas e contêm termos financeiros recorrentes.
2. **A RNN pode sofrer com o tamanho reduzido do corpus**, pois seus embeddings são aprendidos do zero.
3. **Embeddings contextuais + LightGBM podem apresentar bom custo-benefício**, aproveitando conhecimento pré-treinado sem ajustar o encoder.
4. **Fine-Tuning de BERTimbau pode capturar relações de contexto e direcionalidade** sem regras manuais.
5. **LoRA pode aproximar o desempenho do Fine-Tuning completo** treinando uma fração dos parâmetros.
6. **A recuperação pode ajudar especialmente em frases ambíguas**, desde que o índice contenha apenas exemplos do treino.

---

## 17. Lições Práticas

- Não utilizar Accuracy como única métrica, devido à predominância da classe neutra.
- Adotar F1-score Macro como métrica principal.
- Reportar também F1 por classe.
- Impedir que textos duplicados apareçam em partições diferentes.
- Preservar negações, números, percentuais e símbolos financeiros.
- Avaliar separadamente os erros entre neutro e positivo e entre neutro e negativo.
- Comparar modelos sob os mesmos splits.
- Utilizar somente o treino para construir índices de recuperação.
- Selecionar hiperparâmetros apenas com o conjunto de validação.
- Tratar divergências humanas como parte inerente da tarefa, não apenas como ruído.

---

## 18. Síntese Final

A principal mensagem do paper é que, em textos financeiros, o sentimento não está apenas nas palavras, mas nas **relações entre conceitos, direção dos eventos e contexto da frase**.

O trabalho demonstra que a combinação de conhecimento financeiro explícito com aprendizado supervisionado supera abordagens baseadas somente em contagem de termos. Sua contribuição mais duradoura é o Financial PhraseBank, que permite avaliar desde modelos lineares tradicionais até Transformers modernos sob uma tarefa comum de classificação de sentimento financeiro.
