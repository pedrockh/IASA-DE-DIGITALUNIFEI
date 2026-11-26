# DIÁRIO DE LEITURA E SÍNTESE CRÍTICA DA LITERATURA DE APRENDIZADO QUÂNTICO APLICADO A SINAIS DE ELETROCARDIOGRAMA

**Discente:** Pedro Henrique Oliveira Santos  
**Orientação:** Prof. Dr. Rafael Sanchotene Silva  
**Projeto:** Seleção de Atributos e Classificação em ECG via Algoritmo Quantum Snowflake (QSA)  
**Entregável:** Semana 2 — Matriz de Literatura e Diário de Leitura Crítica  
**Norma de Formatação:** ABNT NBR 14724 / NBR 6023 (Monocromático, Texto em Preto)  

---

## 1. INTRODUÇÃO E CONTEXTUALIZAÇÃO DO PLANO DE TRABALHO

O diagnóstico cardiovascular fundamentado na análise de sinais de Eletrocardiograma (ECG) constitui um dos pilares da medicina moderna e da biomedicina computacional. A interpretação manual dos traçados eletrofisiológicos, embora consolidada, apresenta variabilidade interobservador e dependência de especialistas altamente treinados. Nos últimos anos, a aplicação de técnicas de Aprendizado de Máquina (*Machine Learning* - ML) e Aprendizado Profundo (*Deep Learning* - DL) permitiu automatizar a triagem de arritmias com elevada acurácia. Contudo, os modelos estatísticos clássicos enfrentam desafios severos relacionados ao elevado custo computacional, à vulnerabilidade do modelo de "caixa-preta" sem explicabilidade clínica, ao sobreajuste (*overfitting*) decorrente do desbalanceamento de classes e aos riscos de invasão de privacidade e ciberataques em ambientes de Internet das Coisas Médicas (*Medical Internet of Things* - MIoT).

Nesse cenário, a fusão entre a computação quântica e o processamento de sinais biomédicos surge como uma nova fronteira científica. A utilização de princípios fundamentais da mecânica quântica — tais como a superposição de estados, o emaranhamento, o tunelamento em espaços de busca e o mapeamento vetorial em Espaços de Hilbert de alta dimensão — oferece capacidades inéditas de expressividade matemática, eficiência na redução de atributos e segurança cibernética descentralizada.

O presente diário de leitura integra as atividades da **Semana 2** do plano de pesquisa e estabelece a revisão sistemática e comparativa entre a metaheurística base do projeto — o *Quantum Snowflake Algorithm* (QSA) proposto por Oralhan & Oralhan (2025) — e os três estudos mais recentes do estado da arte de 2026 dedicados ao processamento quântico de ECGs (Wu et al., 2026; Salehi et al., 2026; Al-Shamasneh et al., 2026).

---

## 2. ANÁLISE COMPARATIVA DOS QUATRO ARTIGOS CENTRAIS

A literatura selecionada contempla abordagens complementares que cobrem desde a otimização de parâmetros e seleção de atributos até classificadores quânticos avançados e ecossistemas seguros de transmissão biomédica.

### 2.1 Oralhan & Oralhan (2025) — Quantum Snowflake Algorithm (QSA)
* **Objetivo Geral:** Propor uma metaheurística hibridizada inovadora para otimização contínua e discreta de grande escala, inspirada no comportamento físico de flocos de neve na atmosfera e impulsionada por mecânica quântica.
* **Mecanismos Principais:**
  1. *Repulsão por Colisão:* Mantém a diversidade da população no espaço auxiliar contínuo, impedindo o agrupamento prematuro dos indivíduos.
  2. *Tunelamento Quântico:* Realiza saltos estocásticos governados por uma rotina de *quantum annealing* para transpor barreiras de alto valor objetivo e escapar de mínimos locais em superfícies multimodais rugosas.
  3. *Superposição e Partilha de Soluções:* Transmite blocos construtivos promissores (*building blocks*) da melhor solução global para os demais candidatos da população.
  4. *Busca Local e Refinamento:* Executa operadores locais (como a troca 2-Opt no problema do cixeiro viajante) para intensificação e ajuste fino das soluções.
* **Resultados e Desempenho:** No benchmark contínuo de Lévy (30 dimensões), o QSA superou significativamente abordagens consolidadas (ABC, GA, PSO, SA, CS e QPSO) com *fitness* médio de 2,9017 e validação estatística por ANOVA ($F = 704,59; p < 1 	imes 10^{-15}$). No problema discreto do Cixeiro Viajante (TSP com 200 cidades), obteve a rota mínima de 6567,58 unidades de distância.
* **Limitações:** Exige elevado tempo de execução (*wall-clock time*) devido ao re-mapeamento constante entre o espaço auxiliar contínuo e o espaço combinatorial discreto. Não é um classificador estatístico, mas sim um otimizador.

### 2.2 Wu et al. (2026) — Algoritmos Híbridos Quântico-Clássicos para Arritmia em ECG
* **Objetivo Geral:** Avaliar de forma sistemática a viabilidade de modelos híbridos quântico-clássicos para classificação binária de arritmias cardíacas com integração a uma plataforma interativa de auxílio ao diagnóstico (StreamLit).
* **Metodologia e Modelos:** O pipeline utiliza a rede ResNet50 pré-treinada para extração de características visuais de segmentos de ECG (vetores de 2048 dimensões), limpeza por *Isolation Forest* e redução de dimensionalidade via PCA ($k = 5, 8, 12, 16$). Em seguida, avalia três classificadores quânticos: *Quantum Support Vector Classifier* (QSVC via IBM Qiskit com *ZFeatureMap*) e duas Redes Neurais Quânticas (*Estimator-based QNN* e *Sampler-based QNN* via PennyLane).
* **Resultados e Desempenho:** O modelo QSVC ($k=16$ qubits) alcançou o melhor desempenho global na base MIT-BIH, atingindo **97,7% de acurácia, 97,5% de F1-score e 99,4% de ROC-AUC**, superando redes recorrentes clássicas como a RNN-LSTM (88,89%). Sob divisão estrita por paciente (*patient-level split*), o QSVC manteve 91,6% de acurácia. A explicabilidade foi fornecida via protótipos de vizinhança local no espaço de componentes principais.
* **Limitações:** A validação foi conduzida em simuladores quânticos sobre hardware clássico, sem testes sob ruído térmico de dispositivos NISQ reais; a tarefa foi restrita à classificação binária (normal vs. anormal), simplificando a taxonomia clínica complexa.

### 2.3 Salehi et al. (2026) — Quantum Angle-Distance Kernel (QADK)
* **Objetivo Geral:** Desenvolver um kernel quântico leve, estável e diretamente interpretável para classificação de batimentos cardíacos e detecção de anomalias sem a complexidade estrutural de redes profundas.
* **Formulação do Kernel QADK:** O kernel combina dois termos com significado fisiológico direto:
  $$\mathcal{K}_Q(x, y) = lpha \cdot \mathcal{F}(x, y) + eta \cdot \mathcal{D}(x, y)$$
  Onde $\mathcal{F}(x, y) = |\langle \phi(x) | \phi(y) angle|^2$ representa a *fidelidade angular* (alinhamento no Espaço de Hilbert obtido via *swap-tests*) e $\mathcal{D}(x, y) = |\langle \phi(x)|\phi_0angle - \langle \phi(y)|\phi_0angle|^2$ mede a *distância relativa em relação a uma referência de saúde* ($\phi_0$, derivada da mediana de batimentos normais).
* **Resultados e Desempenho:** Na base PTB Diagnostic ECG Database sob 100 validações de Monte-Carlo com separação estrita por paciente (*patient-wise split*), o QADK-SVM obteve **97,74% de AUC-ROC, 93,69% de acurácia e 92,30% de F1-macro**, superando o SVM linear (+3,70 pp) e aproximando-se do desempenho de CNNs profundas (diferença < 1,7 pp) com apenas 11 qubits simulados. Em detecção de anomalias (*One-Class SVM*), atingiu AUC de 88,4%.
* **Limitações:** A construção da matriz de Gram apresenta complexidade computacional $\Theta(n^2)$, exigindo elevado tempo de cálculo clássico para grandes volumes de dados. Apresentou baixo desempenho em agrupamento não supervisionado (ARI $\le 0,0711$), demonstrando ser estritamente discriminativo.

### 2.4 Al-Shamasneh et al. (2026) — Análise de ECG Aprimorada por Quântica em MIoT
* **Objetivo Geral:** Propor um ecossistema completo para a Internet das Coisas Médicas (MIoT) que unifica o diagnóstico inteligente por redes neurais quânticas à proteção de dados por blockchain quântica.
* **Arquitetura em Três Camadas:**
  1. *Camada de Percepção:* Remoção de ruído por wavelets, normalização e segmentação de batimentos.
  2. *Camada de Rede (Blockchain Quântica):* Utiliza funções de hash quânticas baseadas em caminhadas quânticas discretas (*discrete quantum walks*) e protocolo de autenticação de identidade por pares de partículas emaranhadas (estados de Bell). A alteração de 1 bit no sinal produz 45,47% de difusão no hash.
  3. *Camada de Aplicação (QECNN e QGAN):* Emprega kernels convolucionais quânticos treináveis com circuitos quânticos parametrizados (PQC) e uma cabeça de decisão MLP clássica, acompanhada por um módulo QGAN para aumento de dados e mitigação do desbalanceamento de classes.
* **Resultados e Desempenho:** Na base MIT-BIH, o modelo QECNN atingiu **97,2% de acurácia, 96,8% de precisão, 95,9% de sensibilidade e 97,5% de especificidade**. Na base PTB-XL (12 derivações), alcançou **98,1% de acurácia e 0,98 de ROC-AUC**. Testes sob canais de ruído de Kraus (bit-flip, phase-flip, despolarização) demonstraram elevada resiliência do modelo.
* **Limitações:** A simulação integrada de blockchain quântica e redes convolucionais quânticas demanda alto poder computacional no treinamento clássico; a implementação em hardware físico NISQ ainda depende da maturidade das taxas de decoerência dos qubits.

---

## 3. DIFERENCIAÇÃO CONCEITUAL E OPERACIONAL DOS MÉTODOS

A correta condução da pesquisa exige evitar a equalização genérica entre termos quânticos. A tabela abaixo sintetiza a diferenciação conceitual e operacional entre as quatro abordagens:

| Algoritmo / Estudo | Classe de Método | Mecanismo Quântico / Inspirado | Papel na Análise de ECG | Foco Principal |
| :--- | :--- | :--- | :--- | :--- |
| **QSA** (Oralhan & Oralhan, 2025) | Otimização Metaheurística | Tunelamento quântico ($\sigma(t)$), repulsão contínua e superposição de soluções | Seleção de atributos (geração de máscara binária de características) | Eficiência em busca e fuga de mínimos locais |
| **QSVC / QNN** (Wu et al., 2026) | Classificador Híbrido Quântico-Clássico | Mapeamento de características (*ZFeatureMap*), estados em Hilbert e otimização por variacional | Classificação binária de arritmias em imagens/vetores de ECG | Explicabilidade local por protótipos e suporte web |
| **QADK-SVM** (Salehi et al., 2026) | Kernel Quântico Discriminativo | Fidelidade angular ($F$) e distância à referência mediana de saúde ($D$) via *swap-tests* | Classificação de batimentos e detecção de anomalias (*One-Class*) | Representação geométrica leve e transparência matemática |
| **QECNN + QBC** (Al-Shamasneh et al., 2026) | Rede Convolucional Quântica + Blockchain | Kernels convolucionais PQC, *quantum walks* e emaranhamento por pares de Bell | Classificação robusta e transmissão/armazenamento imutável | Segurança cibernética em MIoT e aumento por QGAN |

---

## 4. MAPEAMENTO DE LACUNAS METODOLÓGICAS E CUIDADOS PARA O BENCHMARK

A análise crítica dos artigos selecionados permitiu mapear quatro lacunas e cuidados metodológicos essenciais que nortearão a execução experimental do projeto de pesquisa:

1. **Vazamento de Dados por Particionamento de Amostras (*Data Leakage*):**
   * *Diagnóstico:* Muitos trabalhos na literatura tradicional realizam a divisão aleatória (*random split*) no nível de batimentos individuais. Como múltiplos batimentos pertencem ao mesmo paciente, o modelo "decora" a morfologia específica do indivíduo, gerando acurácias inflacionadas.
   * *Diretriz do Projeto:* O benchmark do QSA deve adotar estritamente o particionamento por paciente (*patient-wise / patient-level split*), garantindo que os pacientes do conjunto de teste jamais tenham sido vistos durante a fase de treinamento ou seleção de atributos.

2. **Comparação Justa com Baseline Clássico (*Fair Benchmarking*):**
   * *Diagnóstico:* Diversos estudos comparam classificadores quânticos avançados com redes neurais clássicas sub-otimizadas ou sem o mesmo pré-processamento minucioso.
   * *Diretriz do Projeto:* O algoritmo QSA (para seleção de atributos) deve ser avaliado contra metaheurísticas bem estabelecidas (como GA, PSO e QPSO) sob as mesmíssimas condições de entrada, avaliando o custo computacional, o tempo de convergência e a dimensão do vetor de características resultante.

3. **Incerteza do Hardware NISQ e Sensibilidade ao Ruído:**
   * *Diagnóstico:* A maioria das pesquisas atuais executa modelos quânticos através de simuladores ideais isentos de ruído em computadores clássicos, o que esconde a degradação de desempenho em dispositivos reais de escala intermediária (*Noisy Intermediate-Scale Quantum* - NISQ).
   * *Diretriz do Projeto:* É necessário incorporar testes de robustez sob canais formais de ruído quântico (como erro de porta, despolarização e bit-flip) para atestar a estabilidade da máscara binária gerada pelo QSA.

4. **Explicabilidade Clínica e Significado Fisiológico:**
   * *Diagnóstico:* A redução drástica de atributos pode eliminar variações morfológicas sutis cruciais para o diagnóstico médico (como alterações na onda P ou no segmento ST).
   * *Diretriz do Projeto:* A avaliação da função de *fitness* do QSA não deve focar unicamente na acurácia matemática, mas também verificar se as ondas essenciais do sinal de ECG (complexo QRS) foram preservadas pelo subconjunto selecionado.

---

## 5. CONCLUSÃO

A revisão sistemática conduzida na Semana 2 demonstra que a introdução de conceitos quânticos em biomedicina atua de forma complementar e não concorrente. Enquanto abordagens como QADK e QECNN inovam na estrutura do classificador e do kernel, o **Quantum Snowflake Algorithm (QSA)** posiciona-se como uma ferramenta de otimização de alta performance para a etapa de **seleção de atributos**. A utilização do QSA para reduzir a dimensionalidade dos dados de ECG antes do envio para modelos preditivos preenche uma lacuna fundamental, unindo a eficiência na busca combinatorial à integridade diagnóstica.

---

## 6. REFERÊNCIAS BIBLIOGRÁFICAS (ABNT NBR 6023)

AL-SHAMASNEH, Ala’a R.; KARIM, Faten Khalid; WANG, Yu. Quantum-enhanced ECG analysis for reliable and secure arrhythmia detection in medical IoT systems. **Journal of Cloud Computing**, v. 15, n. 90, p. 1-30, 2026. DOI: 10.1186/s13677-026-00918-1.

ORALHAN, Zeki; ORALHAN, Burcu. Quantum Snowflake Algorithm (QSA): A Snowflake-Inspired, Quantum-Driven Metaheuristic for Large-Scale Continuous and Discrete Optimization with Application to the Traveling Salesman Problem. **Applied Sciences**, v. 15, n. 9, p. 5117, 2025. DOI: 10.3390/app15095117.

SALEHI, Arman; GOUDARZI, Hamid Reza; HEYDARIAN, Ashkan. Quantum Angle–distance kernel for ECG classification and anomaly detection: a quantum-inspired framework for biomedical signal analysis. **BioData Mining**, v. 19, n. 14, p. 1-24, 2026. DOI: 10.1186/s13040-026-00519-3.

WU, Bolun et al. Quantum-classical hybrid algorithms for ECG arrhythmia detection: a comparative study and application. **BMC Medical Informatics and Decision Making**, v. 26, n. 1, p. 1-22, 2026. DOI: 10.1186/s12911-026-03823-4.
