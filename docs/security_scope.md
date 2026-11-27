# Delimitação do Escopo de Segurança Cibernética e Análise de Sinais de ECG

**Documento do Projeto:** `docs/security_scope.md`  
**Autor:** Pedro Henrique Oliveira Santos  
**Orientação:** Prof. Dr. Rafael Sanchotene Silva  
**Projeto:** Inteligência Artificial Confiável e Segura Aplicada à Saúde Digital — UNIFEI  
**Contexto de Aplicação:** Sistemas de Internet das Coisas Médicas (MIoT) e Diagnóstico por ECG  

---

## 1. Contextualização e Objetivo do Documento

Este documento estabelece a diferenciação metodológica rigorosa entre **análise clínica/diagnóstica de sinais de eletrocardiograma (ECG)** e **detecção de ataques cibernéticos em infraestruturas de saúde digital**. 

No primeiro artigo científico do projeto (*When Does Quantum Snowflake Feature Selection Help?*), o foco primário é a **seleção de atributos e a acurácia diagnóstica sob validação por paciente**. A identificação de ciberataques, manipulações maliciosas e protocolos de segurança em redes MIoT (como a blockchain quântica descrita por Al-Shamasneh et al., 2026) compõe uma camada de pesquisa subsequente. 

Para evitar ambiguidades conceituais entre anomalias fisiológicas e ameaças cibernéticas, definem-se a seguir os cenários de alteração de sinal e as limitações de detecção baseadas exclusivamente na forma de onda do ECG.

---

## 2. Cenários Didáticos de Alteração em Sinais de ECG

Para classificar com precisão as variações observadas em um sinal de ECG coletado por dispositivos vestíveis ou exames clínicos, estabelecem-se três cenários distintos:

```
                  +-------------------------------------------------------+
                  |            TIPO DE ALTERAÇÃO NO SINAL ECG             |
                  +-------------------------------------------------------+
                                              |
      +---------------------------------------+---------------------------------------+
      |                                       |                                       |
      v                                       v                                       v
+---------------------------+   +---------------------------+   +---------------------------+
|   CENÁRIO C0: ARTEFATO    |   |     EVENTO CLÍNICO        |   |   CENÁRIO A1: ATAQUE      |
|  (Ruído Físico/Legítimo)  |   |   (Arritmia Biológica)    |   |  (Adulteração Intencional)|
+---------------------------+   +---------------------------+   +---------------------------+
| - Oscilação de linha base |   | - Contração Ventricular   |   | - Injeção maliciosa de    |
| - Ruído muscular (EMG)    |   |   Prematura (PVC/V-beat)  |   |   falso pulso cardiaco    |
| - Rede elétrica (60 Hz)   |   | - Fibrilação Atrial       |   | - Perturbação adversarial |
| - Mau contato de eletrodo |   | - Bloqueio de Ramo        |   | - Modificação de arquivo  |
+---------------------------+   +---------------------------+   +---------------------------+
| Rótulo: RUÍDO BENIGNO     |   | Rótulo: PATOLOGIA CLÍNICA |   | Rótulo: ATAQUE CIBERNÉTICO|
+---------------------------+   +---------------------------+   +---------------------------+
```

### Cenário C0 — Artefato ou Ruído Legítimo (Físico / Instrumental)
* **Descrição:** Alterações induzidas por processos físicos involuntários do paciente ou limitações do equipamento de medição durante a aquisição do sinal.
* **Exemplos Concretos:**
  1. *Oscilação da Linha de Base (Baseline Wander):* Deriva de baixa frequência (0.15–0.5 Hz) causada pelos movimentos de expansão e contração torácica durante a respiração do paciente.
  2. *Interferência da Rede Elétrica:* Ruído senoidal de alta frequência (50 Hz ou 60 Hz) induzido pelo acoplamento magnético dos cabos do eletrocardiógrafo à rede de energia.
  3. *Artefato Muscular (EMG):* Ruído estocástico de alta frequência resultante da atividade eletromiográfica de músculos esqueléticos em pacientes trêmulos ou em movimento.
  4. *Perda Descontinua de Contato:* Deslocamento mecânico dos eletrodos de gel sobre a pele do paciente.
* **Classificação de Segurança:** **Ruído Benigno / Flutuação Instrumental.** Não há presença de agente malicioso ou intenção adversária. Deve ser mitigado na Camada de Percepção por algoritmos de filtragem (Wavelet, filtros passafaixa) e não deve ser rotulado como ataque.

### Cenário Fisiológico — Evento Clínico (Arritmia / Condição Biológica Verdadeira)
* **Descrição:** Alteração morfológica ou de ritmo cardíaco genuína, gerada pelo sistema de condução elétrica do coração do paciente.
* **Exemplos Concretos:**
  1. *Contração Ventricular Prematura (PVC / V-beat):* Complexo QRS alargado e bizarro seguido de pausa compensatória completa, sem onda P antecedente.
  2. *Fibrilação Atrial (AFIB):* Ausência de ondas P visíveis, substituídas por ondas f irregulares de fibrilação e intervalos R-R totalmente irregulares.
  3. *Bloqueio de Ramo Esquerdo (LBBB):* Duração do complexo QRS superior a 120 ms com entalhe na onda R nas derivações esquerdas.
* **Classificação de Segurança:** **Manifestação Fisiológica / Alvo Diagnóstico.** É a condição clínica real que o modelo de inteligência artificial (QSA + Classificador) deve identificar para auxílio médico. Não possui qualquer relação com segurança de sistemas.

### Cenário A1 — Alteração Intencional Simulada (Ciberataque / Manipulação Maliciosa)
* **Descrição:** Modificação deliberada e maliciosa introduzida no sinal de ECG por um agente adversário com o objetivo de induzir um falso diagnóstico, ocultar uma patologia grave ou comprometer a confiabilidade do sistema médico.
* **Exemplos Concretos:**
  1. *Injeção de Falso Pulso Cardíaco (Signal Injection Attack):* Inserção sintética de batimentos anormais em um sinal de ECG de um paciente saudável para simular uma arritmia fatal e provocar intervenções médicas inadequadas.
  2. *Ataque Adversarial Computacional:* Adição de uma perturbação imperceptível ao olho humano $\delta$, calculada via algoritmos de gradiente (ex.: FGSM), de modo que o sinal $x + \delta$ faça a rede neural classificar uma arritmia grave como um ritmo sinusal normal.
  3. *Adulteração de Arquivos em Trânsito:* Alteração não autorizada de registros médicos transmitidos em redes sem fio não criptografadas entre o sensor IoT e a nuvem.
* **Classificação de Segurança:** **Ataque Cibernético.** Ação adversária deliberada que viola os pilares de **Integridade** e **Autenticidade** da informação em saúde digital.

---

## 3. Justificativa da Rotulagem Estrita de Ataque Cibernético

Apenas o **Cenário A1** recebe o rótulo formal de **Ataque Cibernético**. A justificativa científica e metodológica fundamenta-se nos seguintes aspectos:

1. **Intencionalidade do Agente:** Um ataque cibernético exige a presença de um agente adversário motivado a violar a segurança do sistema. Ruídos de linha de base (C0) e batimentos ectópicos (Evento Clínico) ocorrem naturalmente sem qualquer ação adversária.
2. **Impacto na Tomada de Decisão do Classificador:** Se ruídos instrumentais (C0) ou arritmias verdadeiras fossem rotulados como ciberataques, os sistemas de monitoramento de segurança gerariam uma taxa inaceitável de **falsos alarmes de segurança**, sobrecarregando as equipes de TI e desviando o foco do atendimento médico emergencial.
3. **Mecanismo de Mitigação Distinto:**
   * Ruídos em **C0** são resolvidos por pré-processamento de sinal (filtros digitais, wavelets).
   * Eventos clínicos são encaminhados para decisão médica / triagem de urgência.
   * Ataques do tipo **A1** exigem contramedidas de cibersegurança (blockchain quântica, funções hash quânticas, autenticação por estados de Bell e treinamento adversarial).

---

## 4. Limitações Metodológicas na Detecção do Ataque de Retransmissão (*Replay Attack*)

Um **Ataque de Retransmissão (*Replay Attack*)** consiste na captura não autorizada de um trecho de sinal de ECG autêntico e válido de um paciente (por exemplo, um segmento de ECG pré-gravado indicando ritmo sinusal normal) e sua posterior retransmissão maliciosa para o servidor do hospital no lugar do sinal em tempo real.

```
+------------------+         Interceptação           +------------------+
| Sensor ECG IoT   | ======= Sinal Legítimo =======> | Atacante (Eve)   |
| (Paciente Real)  |                                 | (Gravador)       |
+------------------+                                 +------------------+
                                                              |
                                                    Retransmissão posterior
                                                              v
                                                     +------------------+
                                                     | Servidor do      |
                                                     | Hospital         |
                                                     +------------------+
```

### Por que a Análise Isolada da Forma de Onda falha na Detecção do *Replay Attack*?

1. **Validade Fisiológica Perfeita:** O sinal retransmitido por Eve é um trecho de ECG biologicamente autêntico, coletado de um ser humano real. Portanto, ele possui morfologia perfeita de ondas P, complexos QRS e ondas T, respeitando todos os intervalos fisiológicos ($PR$, $QT$).
2. **Incapacidade do Modelo de IA Diagnóstico:** Qualquer modelo de aprendizado de máquina (seja um SVM clássico, QSVC ou QCNN) treinado para reconhecer padrões morfológicos de ECG analisará o segmento retransmitido e concluirá corretamente que se trata de um ECG válido e saudável. O modelo de forma de onda não tem como saber que o sinal pertence ao passado ou a outro momento do paciente.
3. **Necessidade Incontornável de Metadados de Contexto e Camada de Rede:**
   A detecção de um *replay attack* **não é um problema de reconhecimento de padrões no sinal**, mas sim um problema de **autenticação de protocolo de rede**. Sua prevenção e identificação exigem obrigatoriamente metadados externos à forma de onda, tais como:
   * **Marcas Temporais Criptográficas (*Timestamps* Sincronizados):** Verificação de que o pacote de dados foi gerado dentro de uma janela de tempo válida (ex.: tolerância de milissegundos em relação ao relógio do servidor).
   * **Contadores de Sequência e Valores Aleatórios Únicos (*Nonces*):** Inclusão de um número sequencial não repetível assinado criptograficamente em cada pacote transmitido.
   * **Infraestrutura de Blockchain e Assinatura Quântica:** Utilização de encadeamento quântico por funções hash e autenticação de identidade por pares de Bell (como proposto em Al-Shamasneh et al., 2026), em que cada bloco de dados é vinculado de forma única ao estado da rede no instante exato da transmissão.

---

## 5. Resumo das Implicações para o Projeto de Iniciação Científica

* **Artigo 1 (Benchmark QSA-ECG):** Tratará estritamente da classificação e seleção de atributos para diagnóstico médico, operando sobre dados pré-processados e validados por paciente (Cenário Fisiológico e tolerância ao ruído C0).
* **Trabalhos Futuros (Segurança em MIoT):** Avaliarão a resiliência dos classificadores frente a perturbações adversariais e ataques A1, integrando a pipeline de IA a mecanismos de integridade baseados em blockchain quântica na camada de rede.
