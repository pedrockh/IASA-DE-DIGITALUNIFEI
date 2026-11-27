# Auditoria do Repositório de Referência OQSA
## Oralhan Quantum Snowflake Algorithm (QSA)

**Documento do Projeto:** `docs/code_audit.md`  
**Autor:** Pedro Henrique Oliveira Santos  
**Orientação:** Prof. Dr. Rafael Sanchotene Silva  
**Projeto:** Inteligência Artificial Confiável e Segura Aplicada à Saúde Digital — UNIFEI  
**Repositório de Trabalho:** `IASA-DE-DIGITALUNIFEI`  
**Repositório de Referência:** [github.com/OQSA/OQSA](https://github.com/OQSA/OQSA)  
**Cópia Local Auditada:** `D:\iaconfiavel\OQSA REPOSITORY`  
**Data da Auditoria:** 26 de novembro de 2026  
**Commit Exato:** `4c7b1f21f174b353d7dac5c192fb396d50fc61c4`  
**Ramificação:** `main`  
**Licença do Código no Repositório:** não foi encontrado arquivo formal de licença como `LICENSE` ou `COPYING` no commit auditado.

---

## 1. Visão Geral da Auditoria de Código

Esta auditoria analisa a implementação original do **Oralhan Quantum Snowflake Algorithm (QSA)** disponibilizada pelos autores no GitHub, antes de sua reimplementação para seleção de atributos em sinais de eletrocardiograma (ECG).

A revisão cobre integralmente os seis arquivos presentes no diretório de referência no commit indicado:

- `OQSA_Binary.m`
- `Test_OQSA_Binary.m`
- `tsp.m`
- `OQSA_Generic_Code.ipynb`
- `TSP_OQSA_OTHERS.ipynb`
- `TEST_FUNCTIONS_OQSA_OTHERS.ipynb`

Os principais resultados são:

- **Bug confirmado:** no notebook genérico, uma posição bidimensional é convertida diretamente em uma rota por `np.argsort`, produzindo `[1, 0]` para um problema com 20 cidades.
- **Bug confirmado:** no MATLAB de TSP, `mapPosToRoute` reinicia o gerador aleatório e gera uma permutação aleatória; a função não constitui um mapeamento estável ou significativo da posição contínua.
- **Bug confirmado:** o demo retorna um custo menor após usar uma rota incompleta, mas esse custo não representa uma solução válida para o circuito completo de 20 cidades.
- **Problema confirmado por inspeção:** `cKDTree(...).query(..., k=2)` inclui o próprio agente; portanto, cada consulta contém um único vizinho real para o cálculo de colisão.
- **Limitação metodológica confirmada:** os notebooks fixam número de iterações, não um orçamento uniforme de avaliações da função objetivo, e não apresentam teste estatístico completo para amostras independentes.
- **Hipótese em validação:** a sincronização contínua/discreta no código binário pode prejudicar a busca; a análise indica o risco, mas não uma execução comparativa que o confirme empiricamente.
- **Hipótese em validação:** a ausência de sincronização das posições em outros métodos deve ser confirmada por execução controlada antes de sua classificação como bug.

O commit de referência foi obtido da cópia local. Não foi usada a expressão “commit oficial de publicação do artigo” como substituto para a identificação criptográfica do repositório.

---

## 2. Proveniência e Licenciamento

### 2.1 Código-fonte

O repositório foi auditado na cópia local `D:\iaconfiavel\OQSA REPOSITORY`, cuja origem configurada é `https://github.com/OQSA/OQSA.git`.

O HEAD local é o commit:

`4c7b1f21f174b353d7dac5c192fb396d50fc61c4`

O conteúdo corresponde ao estado da ramificação `main` observado em 26 de novembro de 2026. Esse identificador é distinto do commit anterior, `cab27466a1605043a44669f21db75bdd64554f06`, registrado em 16 de fevereiro de 2025.

### 2.2 Licença

- **Artigo:** o PDF oficial encontrado em `D:\iaconfiavel\alguns docs\applsci-15-05117.pdf` declara: “This article is an open access article distributed under the terms and conditions of the Creative Commons Attribution (CC BY) license (https://creativecommons.org/licenses/by/4.0/)”.
- **Código-fonte:** a cópia auditada não contém `LICENSE`, `COPYING` ou arquivo equivalente no commit `4c7b1f21f174b353d7dac5c192fb396d50fc61c4`.
- **Conclusão:** a licença CC BY 4.0 do artigo não pode ser estendida automaticamente ao código-fonte. A reutilização do código deve considerar a ausência de uma licença explícita no repositório e consultar os autores ou a instituição responsável antes do uso não autorizado.

---

## 3. Mapeamento Geral dos Arquivos

| Arquivo / Script | Ambiente | Finalidade | Entradas e Saídas | Dependências | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `OQSA_Binary.m` e `Test_OQSA_Binary.m` | MATLAB | Variante binária do QSA para minimização discreta. O teste minimiza $f(x)=(\sum x_i)^2$, com $N=30$. | 14 parâmetros posicionais; retorna `bestSol` e `bestCost`. | Funções nativas do MATLAB. | Não executado nesta auditoria; sem saída salva no repositório. |
| `tsp.m` | MATLAB | Demonstração do OQSA em TSP com 200 cidades em um plano $1000 \times 1000$. | Coordenadas geradas internamente; retorna rota e convergência. | `Statistics and Machine Learning Toolbox` (`pdist`, `squareform`). | Não executado nesta auditoria; sem saída salva. |
| `OQSA_Generic_Code.ipynb` | Python 3.9.7 | Implementação genérica para problemas discretos, demonstrada em TSP aleatório com 20 cidades. | Matriz de distâncias e parâmetros; retorna solução e histórico. | `numpy`, `scipy`, `numba`. | Bug confirmado na remapeamento binário-rotacional; saída salva inválida. |
| `TSP_OQSA_OTHERS.ipynb` | Python 3.9 | Benchmark de TSP com 200 cidades comparando OQSA, ABC, GA, PSO e SA. | Coordenadas sintéticas, população de 25 agentes e 50 iterações. | `numpy`, `scipy`, `numba`, `matplotlib`, `scikit-learn`. | Saída parcial e incompleta; PSO e SA não aparecem integralmente. |
| `TEST_FUNCTIONS_OQSA_OTHERS.ipynb` | Python 3.9 | Benchmark contínuo em HyperSphere e Lévy, 30D e 10 execuções. | Sementes 42–51; retorna tabela comparativa e curvas. | `numpy`, `scipy`, `matplotlib`. | Saída completa salva; não remove a limitação de orçamento desigual. |

**Nota de integridade:** “não executado nesta auditoria” significa apenas que o MATLAB não foi executado novamente neste projeto. Isso não invalida eventuais execuções realizadas pelos autores e registradas fora da cópia auditada.

---

## 4. Evidências Reproduzíveis

### 4.1 Notebook genérico

- **Arquivo:** `OQSA_Generic_Code.ipynb`
- **Ambiente salvo:** Python 3.9.7, kernel `python3`.
- **SHA-1 Git do arquivo local:** `5d7989a37d43e52d346e183019932df83c73a801`
- **Cell ID com saída:** `0b9fe4d6`
- **Execution count:** `1`
- **Saída salva:**
  - `Best Solution Found: [1 0]`
  - `Best Cost: 0.7156594837375754`
  - `Cost Evolution: [7.6061894919316355, 0.7156594837375754, ...]`
- **Código relevante:** `init_pos = np.random.rand(pop_size, 2)` cria posições com dimensão 2, enquanto o problema tem 20 cidades; no remapeamento, `base_sol = np.argsort(positions[i])`.
- **Classificação:** bug confirmado.

### 4.2 Notebook comparativo de TSP

- **Arquivo:** `TSP_OQSA_OTHERS.ipynb`
- **SHA-1 Git do arquivo local:** `9328adcc1ec2553c9592492f2cf9c359a4fc8fcf`
- **Código relevante:** a posição é calculada por `positions = np.array([cities[ind].mean(axis=0) for ind in population])`.
- **Classificação:** hipótese metodológica apoiada pela definição matemática. Como todas as permutações contêm as mesmas cidades, seus centroides deveriam ter a mesma média global; a posição deixa de representar a ordem da rota e pode reduzir o significado físico das colisões.

### 4.3 Código MATLAB de TSP

- **Arquivo:** `tsp.m`
- **Linhas relevantes:** função `mapPosToRoute`, aproximadamente linhas 222–237.
- **Código relevante:** `rng(seedVal, 'twister')` e `newRoute = randperm(nCities)`.
- **Classificação:** bug confirmado. A conversão ignora a posição como orientação estável da rota e usa um sorteio aleatório determinado por um hash simples da posição.

### 4.4 Nota sobre o SHA dos notebooks

Os SHA-1 acima são hashes Git dos arquivos locais. Eles permitem verificar que o arquivo analisado corresponde ao conteúdo armazenado no repositório local. O commit exato continua sendo `4c7b1f21f174b353d7dac5c192fb396d50fc61c4`.

---

## 5. Bugs Confirmados e Limitações

### 5.1. Tour degenerado no notebook genérico

`init_pos` possui formato $(P, 2)$, mas a população representa um problema com 20 cidades. Cada `np.argsort(positions[i])` retorna apenas os índices correspondentes às duas colunas da matriz de posição.

A função objetivo de TSP então calcula:

$$C(r)=\sum_{i=0}^{n-1} d(r_i,r_{(i+1)\bmod n})$$

Como $r=[1,0]$ contém apenas duas cidades, o algoritmo calcula apenas um circuito de tamanho 2 em vez de um circuito válido de tamanho 20.

A redução de `7.6061894919316355` para `0.7156594837375754` não demonstra melhoria na solução. O custo é menor porque o domínio efetivamente comparado mudou para um problema incompleto. Essa solução deve ser rejeitada como inválida.

### 5.2. Remapeamento geométrico inadequado no TSP

O notebook comparativo calcula o centroide médio de todas as cidades contidas em cada permutação. Como uma permutação válida usa o conjunto completo das cidades, duas rotas diferentes possuem o mesmo centroide.

Consequência: a posição não diferencia agentes, o que enfraquece ou elimina o significado das forças de colisão e de atração. O MATLAB usa como alternativa um sorteio aleatório em `mapPosToRoute`, passando a atuar como permutação aleatória acoplada a busca local.

### 5.3. Vizinho próprio na cKDTree

Quando `cKDTree(positions).query(positions, k=2)` é usado sem remover o próprio ponto, o primeiro resultado é o próprio agente, com distância zero. O segundo resultado pode ser outro agente, mas a operação `k=2` não fornece dois vizinhos reais.

Para obter dois vizinhos reais, o algoritmo deveria consultar $k=3$ ou remover o índice próprio dos resultados, e a configuração deveria ser definida de acordo com o tamanho da população.

### 4. Orçamento de Avaliações Desequilibrado e Ausência de Testes Estatísticos
* **Problema:** Nos notebooks de benchmark (`TSP_OQSA_OTHERS.ipynb` e `TEST_FUNCTIONS_OQSA_OTHERS.ipynb`), os algoritmos comparados (OQSA, GA, PSO, ABC, SA) são executados fixando apenas o número de iterações ($MaxIter = 50$).
* **Consequência:** O Simulated Annealing (SA) realiza cerca de 50 avaliações de função objetivo, enquanto o OQSA e o ABC realizam centenas a milhares de avaliações adicionais por iteração (devido aos laços de busca local e exploração da população). Afirmar a "superioridade" do QSA sem igualar o orçamento total de chamadas à função objetivo ($F$-evals) e sem aplicar testes estatísticos não paramétricos (ex.: Wilcoxon, Friedman) invalida a comparação metodológica.

### 5.5. Reprodutibilidade incompleta

O MATLAB usa `rng('shuffle')` em pontos internos de execução, substituindo uma sequência determinística por uma origem baseada no relógio. Para reprodutibilidade, o experimento deve registrar uma semente única no início e passar essa semente a todas as rotinas, sem reinicializar o gerador em cada iteração.

---

## 6. Hipóteses Ainda Não Confirmadas por Experimento

As afirmações abaixo seguem da leitura estática do código, mas exigem execução controlada antes de serem tratadas como bugs confirmados:

1. A posição contínua binária nem sempre é sincronizada com a máscara discreta após uma solução aceita.
2. A soma de forças de colisão pode descalibrar a posição do agente em problemas binários de alta dimensão.
3. A atualização de `Pos` em `OQSA_Binary.m` pode degradar a busca sem ser detectada apenas pela inspeção estática.

Essas hipóteses devem ser testadas com unit tests que comparem máscara, posição embutida, função objetivo e estado retornado a cada iteração.

---

## 7. Representação Binária

Para aplicação em seleção de atributos ECG, uma solução candidata será uma máscara $x \in \{0,1\}^N$, em que:

- $x_i=1$ inclui o $i$-ésimo atributo;
- $x_i=0$ exclui o $i$-ésimo atributo.

A versão reimplementada deve manter explicitamente:

$$x_i =
\begin{cases}
1, & \text{se } p_i \geq 0.5,\\
0, & \text{caso contrário},
\end{cases}$$

em que $p_i$ representa a probabilidade contínua associada ao bit $i$. Sempre que uma máscara aceitar uma melhoria, sua posição contínua correspondente deverá ser atualizada ou reconstruída, evitando estados discordantes.

---

## 8. Diretrizes para a Reimplementação

1. Implementar a versão binária em Python 3.12 com tipagem estática e testes unitários.
2. Exigir sincronização entre máscara discreta e vetor contínuo em toda transição de estado.
3. Usar o mesmo orçamento exato de $F$-evals para QSA, GA, PSO, ABC, SA e demais baselines.
4. Executar pelo menos 30 repetições independentes por experimento e reportar média, desvio-padrão, mediana e intervalo de confiança.
5. Aplicar validação independente por paciente (*patient-wise split*), mantendo o treinamento e o ajuste de hiperparâmetros estritamente dentro da pasta de treino.
6. Avaliar acurácia, sensibilidade, especificidade, F1-score e quantidade de atributos.
7. Fixar uma semente global registrada em arquivo de configuração e salvar todas as permutações e resultados por execução.
8. Aplicar testes estatísticos não paramétricos apenas após confirmar independência das execuções e risco controlado de vazamento de dados.
