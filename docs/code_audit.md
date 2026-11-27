# Auditoria do Repositório de Referência OQSA (Oralhan Quantum Snowflake Algorithm)

**Documento do Projeto:** `docs/code_audit.md`  
**Autor:** Pedro Henrique Oliveira Santos  
**Orientação:** Prof. Dr. Rafael Sanchotene Silva  
**Projeto:** Inteligência Artificial Confiável e Segura Aplicada à Saúde Digital — UNIFEI  
**Repositório de Trabalho:** `IASA-DE-DIGITALUNIFEI`  
**Repositório de Referência Consultado:** [https://github.com/OQSA/OQSA](https://github.com/OQSA/OQSA)  
**Data da Consulta:** 26 de novembro de 2026  
**Versão/Commit de Referência:** Commit oficial de publicação do artigo (março de 2025)  
**Licença Identificada:** Ausência de arquivo formal de licença (`LICENSE`) no repositório GitHub; o artigo publicado na *Applied Sciences* (Oralhan & Oralhan, 2025) é distribuído sob licença Creative Commons CC BY 4.0.

---

## 1. Visão Geral da Auditoria de Código

Esta auditoria analisa a estrutura de código original do **Oralhan Quantum Snowflake Algorithm (QSA)** disponibilizada pelos autores no GitHub. O objetivo é registrar a proveniência, avaliar a funcionalidade dos scripts disponíveis, mapear dependências e identificar inconsistências técnicas ou limitações metodológicas antes da reimplementação e adaptação do algoritmo para a seleção de atributos em sinais de eletrocardiograma (ECG).

Foram analisados **6 arquivos de código** no repositório de referência, que correspondem a **5 rotinas/experimentos distintos** (sendo um script MATLAB de teste e seu respectivo arquivo de função).

---

## 2. Mapeamento Geral e Registro de Status dos Arquivos

| Arquivo / Script | Linguagem / Ambiente | Finalidade do Código | Entradas e Saídas | Dependências / Toolboxes | Status de Execução / Saída Salva |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`OQSA_Binary.m`** e **`Test_OQSA_Binary.m`** | MATLAB | Implementação da variante binária do QSA para otimização em espaços discretos. O script de teste minimiza $f(x) = (\sum x_i)^2$ para $N=30$ bits. | **Entradas:** 14 parâmetros posicionais (`CostFunction`, `nVar`, `MaxIter`, `P`, `T0`, `alphaT`, `Sigma0`, `alphaS`, `delta`, `betaTunnel`, `gamma`, `pSuper`, `maxNoImprove`, `kLocalSearch`).<br>**Saídas:** `bestSol` e `bestCost`. | Funções nativas do MATLAB. Requer ambos os arquivos no mesmo diretório. | **Não verificado** (Sem saída salva no repositório original; requer ambiente MATLAB ativo). |
| **`tsp.m`** (`OQSA_TSP_Example`) | MATLAB | Demonstração do OQSA na resolução do Problema do Caixeiro Viajante (TSP) com 200 cidades distribuídas em um plano $1000 \times 1000$. | **Entradas:** Geradas internamente com `rng('shuffle')`.<br>**Saídas:** Rota otimizada e gráfico de convergência. | *Statistics and Machine Learning Toolbox* (`pdist`, `squareform`). | **Não verificado** (Sem saída salva no repositório). |
| **`OQSA_Generic_Code.ipynb`** | Python 3.9 | Função `oqsa(...)` genérica para problemas discretos em Python, demonstrada com um TSP de 20 cidades e matriz de distâncias aleatória. | **Entradas:** Matriz de distâncias e parâmetros do algoritmo.<br>**Saídas:** `(melhor_solucao, historico_custo)`. | `numpy`, `scipy` (`cKDTree`), `numba`. | **Executou com Erro Lógico / Resultado Inválido** (Possui saída salva, mas a solução gerada é uma rota degenerada de 2 elementos). |
| **`TSP_OQSA_OTHERS.ipynb`** | Python 3.9 | Benchmark comparativo entre OQSA, ABC, GA, PSO e SA em um TSP de 200 cidades (25 agentes, 50 iterações, `seed=42`). | **Entradas:** Coordenadas e distâncias geradas sinteticamente.<br>**Saídas:** Tabela de custos/tempos e gráficos de rota e convergência. | `numpy`, `scipy`, `numba`, `matplotlib`, `scikit-learn` (importado sem uso). | **Saída Parcial / Incompleta** (Mostra resultados salvos para OQSA, ABC e GA; PSO e SA foram cortados ou interrompidos). |
| **`TEST_FUNCTIONS_OQSA_OTHERS.ipynb`** | Python 3.9 | Avaliação do OQSA em funções de teste contínuas multimodais (*HyperSphere* e *Lévy*) em 30D com 10 execuções independentes. | **Entradas:** Parâmetros de busca no intervalo $[-5.12, 5.12]$, sementes 42 a 51.<br>**Saídas:** Tabela comparativa (Best/Worst/Avg/Std) e curvas de convergência. | `numpy`, `scipy`, `matplotlib`. | **Saída Completa Salva** (Notebook totalmente executado e verificado; demonstra menor custo médio do OQSA na esfera). |

---

## 3. Análise da Representação de Soluções Binárias

A representação binária no QSA original é estruturada no arquivo `OQSA_Binary.m`. No contexto do projeto de seleção de atributos em ECG, a solução candidata é mapeada como um vetor booleano (máscara binária) $x \in \{0, 1\}^N$, em que $N$ é o número total de atributos extraídos do sinal de ECG:

* **Significado dos Bits:** $x_i = 1$ indica a inclusão do $i$-ésimo atributo no subconjunto de dados de entrada do classificador; $x_i = 0$ indica a sua exclusão.
* **Mecanismo Interno de Conversão:** O algoritmo mantém posições contínuas internas $p_i \in [0, 1]$ para os agentes (flocos de neve). A conversão para a máscara discreta é realizada por limiarização probabilística ou por função sigmoide:
  $$x_i = \begin{cases} 1, & \text{se } \text{sigmoide}(p_i) > 0.5 \quad (\text{ou } p_i > \text{rand}()) \\ 0, & \text{caso contrário} \end{cases}$$
* **Inconsistência Observada na Implementação Binária:** No código `OQSA_Binary.m`, quando uma nova solução discreta é aceita por ter melhor *fitness*, as posições contínuas $p_i$ nem sempre são sincronizadas de forma consistente com a solução binária $x_i$. Além disso, as forças de colisão somam $P-1$ vetores unitários contínuos, o que pode descalibrar a escala do intervalo $[0, 1]$ e prejudicar a convergência em problemas binários de alta dimensão.

---

## 4. Auditoria Detalhada: Inconsistências Técnicas e Pontos de Atenção (Bugs e Limitações)

A análise do código-fonte revelou cinco pontos críticos de atenção que devem ser corrigidos na versão reescrita para o benchmark de ECG:

### 1. Resultado Inválido e Tour Degenerado no Notebook Genérico (`OQSA_Generic_Code.ipynb`)
* **Problema:** Na célula de demonstração do TSP no notebook genérico, o remapeamento de posições contínuas para rotas discretas utiliza o comando `np.argsort(positions[i])`. Contudo, o array `positions` foi inicializado com formato $(P, 2)$ (apenas 2 colunas de coordenadas).
* **Consequência:** `np.argsort` ordena apenas 2 elementos por linha, gerando uma "rota" de 2 cidades `[1, 0]` para um problema definido com 20 cidades. O algoritmo reporta um custo de `0.7157` (contra `7.6` do custo inicial), mas o resultado é uma solução fisicamente impossível e degenerada que parece superior apenas por calcular a distância entre 2 pontos em vez do circuito completo de 20 cidades.

### 2. Mapeamento de Posições Inválido no TSP (`tsp.m` e `TSP_OQSA_OTHERS.ipynb`)
* **Problema:** Em `tsp.m` e no notebook comparativo de TSP, a posição contínua atribuída a uma permutação/rota é calculada como o centroide (média geométrica) de todas as coordenadas das cidades visitadas na rota.
* **Consequência:** Como qualquer permutação válida visita exatamente o mesmo conjunto de cidades, o centroide de todas as rotas é matematicamente idêntico. Dessa forma, as forças de colisão e a atração gravitacional quântica perdem o sentido físico, pois a distância espacial entre os agentes no subespaço de posições se torna zero. No próprio código `tsp.m`, a função `mapPosToRoute` gera uma nova rota aleatória por sorteio, atuando como um *placeholder* que faz o algoritmo se comportar como uma busca aleatória acoplada à busca local (2-opt).

### 3. Vizinhança da cKDTree Configurada Incorretamente (`neighbor_k = 2`)
* **Problema:** Nas buscas de vizinhança espacial via `scipy.spatial.cKDTree(positions).query(pos, k=2)`, a consulta é realizada passando as próprias posições dos agentes ativos.
* **Consequência:** Como o próprio ponto do agente está presente na árvore, o primeiro vizinho retornado ($k=1$) é obrigatoriamente o próprio agente com distância 0. Assim, ao solicitar $k=2$, o algoritmo obtém apenas **1 vizinho real**, e não 2 como pretendido originalmente para as operações de superposição/interferência.

### 4. Orçamento de Avaliações Desequilibrado e Ausência de Testes Estatísticos
* **Problema:** Nos notebooks de benchmark (`TSP_OQSA_OTHERS.ipynb` e `TEST_FUNCTIONS_OQSA_OTHERS.ipynb`), os algoritmos comparados (OQSA, GA, PSO, ABC, SA) são executados fixando apenas o número de iterações ($MaxIter = 50$).
* **Consequência:** O Simulated Annealing (SA) realiza cerca de 50 avaliações de função objetivo, enquanto o OQSA e o ABC realizam centenas a milhares de avaliações adicionais por iteração (devido aos laços de busca local e exploração da população). Afirmar a "superioridade" do QSA sem igualar o orçamento total de chamadas à função objetivo ($F$-evals) e sem aplicar testes estatísticos não paramétricos (ex.: Wilcoxon, Friedman) invalida a comparação metodológica.

### 5. Reprodutibilidade Comprometida por Gestão Irregular de Sementes
* **Problema:** O código MATLAB `tsp.m` reinicia o gerador de números aleatórios com `rng('shuffle')` dentro de funções internas executadas a cada iteração (`mapPosToRoute`).
* **Consequência:** Essa prática destrói a sequência aleatória global e impede a reprodutibilidade dos experimentos, tornando impossível replicar exatamente a mesma trajetória de busca sob diferentes execuções.

---

## 5. Diretrizes para a Reimplementação no Projeto (Benchmark de ECG)

Para sanar todas as limitações auditadas e garantir reprodutibilidade científica na seleção de atributos em sinais de ECG, o projeto adotará os seguintes protocolos rigorosos:

1. **Código Autoral em Python 3.12:** Reescrita completa da classe OQSA binária em Python com tipagem estática e testes unitários.
2. **Sincronização de Estados:** Manutenção rigorosa de correspondência entre o vetor contínuo de probabilidades e a máscara binária $x \in \{0, 1\}^N$.
3. **Igualdade Estrita de Orçamento Computacional:** Todas as metaheurísticas comparadas (QSA, GA, PSO, Seleção Aleatória) serão limitadas ao mesmo número exato de $F$-evals (ex.: $5.000$ avaliações da função objetivo).
4. **Validação Independente por Paciente (*Patient-Wise Split*):** A avaliação do *fitness* (acurácia do classificador KNN/SVM + penalidade de número de atributos) será realizada estritamente dentro da pasta de treino do paciente, evitando vazamento de dados (*data leakage*).
5. **Reprodutibilidade Total:** Fixação explícita de sementes aleatórias globais (`seed = 42`) e salvamento das matrizes de permutação.
