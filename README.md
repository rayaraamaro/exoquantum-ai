# ExoQuantum AI

**ExoQuantum AI** é um projeto acadêmico desenvolvido para a **Global Solution da FIAP**, na disciplina de **Inteligência Artificial & Machine Learning**, ministrada pelo professor **Danilo Rodrigues de Assis Elias**.

O projeto utiliza dados astronômicos reais, modelos clássicos de Machine Learning, Deep Learning com MLP e Autoencoder, além de uma etapa experimental com Quantum Machine Learning usando Qiskit e IBM Quantum Runtime.

A proposta principal é investigar como técnicas de IA podem apoiar a classificação e priorização de objetos observados no espaço, especialmente diferenciando candidatos a exoplanetas, planetas confirmados e falsos positivos.

---

## Integrantes

| Nome | RM |
|---|---|
| Erick Molina | 553852 |
| Felipe Castro Salazar | 553464 |
| Marcelo Vieira de Melo | 552953 |
| Rayara Amaro Figueiredo | 552635 |
| Victor Rodrigues | 554158 |

---

## Objetivo do projeto

O objetivo do projeto é construir uma solução de IA/ML capaz de analisar dados da missão Kepler e apoiar a classificação de objetos astronômicos nas seguintes classes:

- `CANDIDATE`: objeto candidato a exoplaneta;
- `CONFIRMED`: planeta confirmado;
- `FALSE POSITIVE`: falso positivo.

Além da classificação tradicional, o projeto também explora uma abordagem experimental com computação quântica, conectando representações aprendidas por Autoencoder com Quantum Kernel Training.

---

## Contexto espacial

A identificação de exoplanetas é uma área importante da astronomia moderna. Missões como Kepler observaram variações no brilho de estrelas para detectar possíveis trânsitos planetários.

Quando um objeto passa na frente de uma estrela, ele pode causar uma pequena queda no brilho observado. Esse fenômeno pode indicar a presença de um planeta, mas também pode ser causado por outros eventos astronômicos, como sistemas binários eclipsantes ou ruídos de observação.

Por isso, nem todo sinal detectado representa um planeta real. O desafio está em diferenciar candidatos promissores de falsos positivos, ajudando na priorização de análises futuras.

---

## Pergunta de pesquisa

No contexto da análise de candidatos a exoplanetas da missão Kepler, é mais crítico classificar um falso positivo como candidato promissor ou descartar um candidato que poderia representar um exoplaneta real?

Essa pergunta orienta a análise dos modelos para além da accuracy, destacando a importância de observar os tipos de erro cometidos, principalmente em relação à classe `CANDIDATE`.

---

## Dataset utilizado

O projeto utiliza o dataset de objetos de interesse da missão Kepler, conhecido como KOI, ou Kepler Objects of Interest.

A base contém informações sobre objetos observados pela missão, incluindo características do trânsito, propriedades estimadas do possível planeta, dados da estrela hospedeira e a classificação final do objeto.

A variável-alvo usada no projeto é:

```text
koi_disposition
```

Ela representa a classificação do objeto como `CANDIDATE`, `CONFIRMED` ou `FALSE POSITIVE`.

---

## Justificativa da escolha do dataset

O dataset da missão Kepler foi escolhido por estar diretamente relacionado ao tema espacial e por permitir a aplicação de técnicas de IA em um problema científico real.

A base é adequada para o projeto porque contém variáveis observacionais e físicas ligadas à detecção de possíveis exoplanetas, além de possuir uma variável-alvo clara para classificação.

Outro ponto importante é que o problema apresenta desafios reais de Machine Learning, como classes desbalanceadas, valores ausentes, outliers e sobreposição entre categorias, principalmente na classe `CANDIDATE`.

---

## Principais variáveis analisadas

Entre as variáveis utilizadas no projeto, destacam-se:

| Variável | Descrição |
|---|---|
| `koi_period` | Período orbital estimado do objeto |
| `koi_duration` | Duração do trânsito observado |
| `koi_depth` | Profundidade da queda de brilho durante o trânsito |
| `koi_num_transits` | Número de trânsitos observados |
| `koi_model_snr` | Relação sinal-ruído do modelo |
| `koi_prad` | Raio estimado do possível planeta |
| `koi_teq` | Temperatura de equilíbrio estimada |
| `koi_insol` | Insolação recebida pelo objeto |
| `koi_steff` | Temperatura efetiva da estrela hospedeira |
| `koi_slogg` | Gravidade superficial da estrela |
| `koi_srad` | Raio da estrela |
| `koi_smass` | Massa da estrela |
| `koi_kepmag` | Magnitude observada pelo Kepler |

Colunas de identificação, como `kepid` e `kepoi_name`, foram mantidas para rastreabilidade, mas não foram usadas como variáveis principais de treinamento.

---

## Estrutura do projeto

```text
exoquantum-ai/
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
│   ├── 01_dataset_validation.ipynb
│   ├── 02_exploratory_analysis.ipynb
│   ├── 03_machine_learning_baselines.ipynb
│   ├── 04_deep_learning_and_autoencoder.ipynb
│   └── 05_quantum_machine_learning.ipynb
├── README.md
└── .gitignore
```

---

## Organização dos notebooks

### 01 — Dataset validation

O primeiro notebook valida a base escolhida, verifica o tamanho do dataset, identifica a variável-alvo, analisa a distribuição das classes e seleciona um conjunto inicial de variáveis relevantes.

Também são analisados valores ausentes e possíveis riscos de `data leakage`.

### 02 — Exploratory analysis

O segundo notebook realiza a análise exploratória dos dados.

Foram analisadas:

- matriz de correlação;
- variáveis principais por classe;
- distribuições com boxplots;
- estatísticas descritivas;
- presença de outliers;
- possíveis relações entre variáveis físicas e observacionais.

Essa etapa ajudou a entender quais variáveis poderiam contribuir para diferenciar `CANDIDATE`, `CONFIRMED` e `FALSE POSITIVE`.

### 03 — Machine Learning baselines

O terceiro notebook treina modelos clássicos de Machine Learning.

Foram testados:

- Logistic Regression;
- Random Forest;
- SVM.

Antes do treinamento, foram aplicadas etapas de preparação dos dados, incluindo:

- separação de variáveis de entrada e alvo;
- tratamento de valores ausentes com mediana;
- codificação da variável-alvo;
- separação entre treino e teste;
- normalização das variáveis.

O modelo com melhor desempenho geral foi o `Random Forest`, com accuracy aproximada de 77,78%.

### 04 — Deep Learning and Autoencoder

O quarto notebook explora técnicas de Deep Learning.

Foram treinadas:

- uma MLP simples;
- uma MLP com `class_weight`;
- um Autoencoder.

A MLP simples teve desempenho inferior ao Random Forest, mas superior a alguns modelos clássicos. Já a MLP com `class_weight` melhorou o recall da classe `CANDIDATE`, mostrando um trade-off entre desempenho geral e sensibilidade para candidatos.

O Autoencoder foi usado para reduzir as 15 variáveis originais para 4 variáveis latentes. Essa representação compacta foi usada posteriormente no experimento de Quantum Machine Learning.

### 05 — Quantum Machine Learning

O quinto notebook conecta Deep Learning e computação quântica.

A etapa quântica foi construída com Qiskit e inspirada no fluxo de Quantum Kernel Training da IBM Quantum.

O fluxo principal foi:

```text
Autoencoder
↓
Espaço latente com 4 variáveis
↓
Feature map quântico
↓
Circuito de overlap
↓
Matriz de kernel quântico
↓
SVM com kernel pré-computado
```

Também foi realizada uma execução real em hardware IBM Quantum usando o backend `ibm_marrakesh`.

A execução em simulador local retornou probabilidade de aproximadamente 0,2559 para o estado `0000`, enquanto a execução no hardware real retornou aproximadamente 0,2080.

Essa diferença é esperada, pois o simulador representa um ambiente ideal, enquanto o hardware real está sujeito a ruídos e limitações físicas.

---

## Resultados principais

| Etapa | Resultado principal |
|---|---|
| Logistic Regression | Baseline inicial, com dificuldade na classe `CANDIDATE` |
| Random Forest | Melhor modelo clássico geral |
| SVM clássico | Resultado inferior ao Random Forest |
| MLP | Aprendeu padrões relevantes, mas não superou o Random Forest |
| MLP com class weight | Melhorou recall de `CANDIDATE`, mas reduziu accuracy geral |
| Autoencoder | Reduziu 15 variáveis para 4 variáveis latentes |
| Quantum Kernel | Gerou matriz de similaridade quântica usada em SVM |
| IBM Quantum Runtime | Validou execução real de circuito no backend `ibm_marrakesh` |

---

## Interpretação dos resultados

O Random Forest apresentou o melhor desempenho geral entre os modelos tradicionais e permaneceu como principal referência do projeto.

A etapa de Deep Learning mostrou que redes neurais podem aprender padrões relevantes, mas não necessariamente superam modelos clássicos em datasets tabulares de tamanho moderado.

O Autoencoder teve papel importante porque criou uma representação compacta dos dados. Essa representação foi essencial para viabilizar a etapa quântica.

A etapa de Quantum Machine Learning deve ser interpretada como uma prova de conceito. Ela mostrou que é possível transformar dados astronômicos em circuitos quânticos, calcular similaridades entre amostras e usar uma matriz de kernel quântico em um modelo de Machine Learning.

---

## Por que usar Quantum Machine Learning?

A computação quântica foi usada neste projeto como uma etapa experimental para investigar novas formas de representar relações entre dados.

Em modelos baseados em kernel, como o SVM, a classificação depende de medidas de similaridade entre amostras. No Quantum Kernel, essa similaridade é calculada depois que os dados são codificados em circuitos quânticos.

No contexto espacial, esse tipo de abordagem pode ser relevante futuramente para problemas com grande volume de dados, eventos raros e padrões difíceis de separar com métodos clássicos.

Neste projeto, a parte quântica não substitui os modelos clássicos. Ela funciona como uma investigação complementar e uma ponte entre IA, astronomia e computação quântica.

---

## Limitações do projeto

Algumas limitações importantes devem ser consideradas:

- O dataset utilizado é tabular e possui menos de 10 mil registros, o que favorece modelos clássicos como Random Forest.
- A classe `CANDIDATE` apresentou maior dificuldade de classificação.
- Os outliers foram analisados, mas não removidos automaticamente, pois podem representar fenômenos reais.
- O experimento quântico foi feito com uma amostra pequena, devido ao custo computacional de calcular kernels quânticos.
- A execução em hardware real está sujeita a ruído, fila e disponibilidade dos backends IBM Quantum.
- A etapa quântica deve ser interpretada como prova de conceito, não como evidência definitiva de superioridade sobre métodos clássicos.

---

## Segurança e IBM Quantum Token

Para executar a etapa opcional com IBM Quantum Runtime, é necessário possuir uma conta na IBM Quantum Platform e um token de acesso.

O token pode ser obtido na área de conta da IBM Quantum Platform.

Por segurança, o token não deve ser enviado ao GitHub.

---

## Como executar o projeto

### 1. Clonar o repositório

```bash
git clone https://github.com/rayaraamaro/exoquantum-ai.git
cd exoquantum-ai
```

### 2. Criar ambiente virtual

```bash
python -m venv .venv
```

### 3. Ativar ambiente virtual

No macOS/Linux:

```bash
source .venv/bin/activate
```

No Windows:

```bash
.venv\\Scripts\\activate
```

### 4. Instalar dependências principais

```bash
pip install pandas numpy matplotlib seaborn scikit-learn tensorflow qiskit qiskit-aer qiskit-machine-learning qiskit-ibm-runtime python-dotenv
```

### 5. Executar os notebooks

A ordem recomendada é:

```text
01_dataset_validation.ipynb
02_exploratory_analysis.ipynb
03_machine_learning_baselines.ipynb
04_deep_learning_and_autoencoder.ipynb
05_quantum_machine_learning.ipynb
```

---

## Próximas melhorias

Possíveis melhorias futuras incluem:

- testar novas arquiteturas de MLP;
- aplicar validação cruzada;
- testar outras técnicas de tratamento de outliers;
- comparar diferentes feature maps quânticos;
- ampliar a mini matriz de kernel quântico;
- testar técnicas de mitigação de erro em hardware real;
- criar uma interface web para visualização dos resultados;
- desenvolver um sistema de priorização de candidatos a exoplanetas.

## Aplicabilidade do projeto

Este projeto pode ser interpretado como uma solução de apoio à decisão para a priorização de candidatos a exoplanetas. Em vez de substituir a análise de especialistas, os modelos desenvolvidos podem funcionar como uma etapa preliminar de triagem, indicando quais objetos possuem características mais próximas de planetas confirmados e quais apresentam maior risco de serem falsos positivos.

A principal utilidade da solução está em apoiar a organização de grandes volumes de dados astronômicos, destacando candidatos que merecem investigação mais detalhada. Além disso, a análise das matrizes de confusão permite avaliar quais tipos de erro são mais frequentes, principalmente em relação à classe `CANDIDATE`.

---

## Referências principais

- NASA Exoplanet Archive.
- Kepler Mission.
- IBM Quantum Documentation.
- Qiskit Documentation.
- Qiskit Machine Learning.
- Scikit-learn Documentation.
- TensorFlow/Keras Documentation.

---

## Observação final

Este projeto combina dados reais de astronomia com técnicas de Machine Learning, Deep Learning e Quantum Machine Learning.

O objetivo não é afirmar que computação quântica supera os métodos clássicos neste cenário, mas demonstrar uma investigação prática, honesta e tecnicamente consistente sobre como essas abordagens podem se conectar para apoiar problemas científicos espaciais.
