# Simulador Numérico UGS (Underground Gas Storage) - 2D IMPES/IMPEC

![Status](https://img.shields.io/badge/Status-Protótipo%20funcional-brightgreen)
![Linguagem](https://img.shields.io/badge/Linguagem-C++-blue)
![Pós-processamento](https://img.shields.io/badge/Pós--processamento-Python-yellow)

## 📌 Sobre o Projeto

Este repositório contém o código-fonte de um simulador numérico de reservatórios bidimensional (2D vertical) desenvolvido para modelar o **Armazenamento Subterrâneo de Gás Natural (UGS)** em aquíferos salinos profundos.

O diferencial do simulador é o acoplamento entre o escoamento bifásico água-gás e o transporte de metano dissolvido na fase aquosa, permitindo avaliar simultaneamente:

- distribuição de pressão;
- saturação de gás;
- inventário de gás livre;
- inventário de metano dissolvido;
- efeitos gravitacionais;
- influência de heterogeneidade petrofísica;
- resposta a ciclos de injeção, repouso e produção.

O código foi desenvolvido como parte de um Trabalho de Conclusão de Curso (TCC) em Engenharia Mecânica na UERJ – Instituto Politécnico.

---

## 🎯 Objetivo

Desenvolver um simulador numérico capaz de representar a dinâmica interna de estocagem de gás em aquíferos, incluindo os principais mecanismos físicos que afetam a recuperação ao longo de ciclos operacionais:

- escoamento bifásico água-gás;
- segregação gravitacional;
- dissolução de metano na água;
- transporte advectivo-difusivo do metano dissolvido;
- influência de porosidade e permeabilidade.

---

## ⚙️ Física Implementada

O simulador trabalha com três variáveis principais de estado:

- **Pressão da fase aquosa** (`p`)
- **Saturação de gás** (`Sg`)
- **Concentração de metano dissolvido** (`C`)

### Mecanismos considerados

- **Escoamento bifásico água-gás**
- **Segregação gravitacional** por diferença de densidades entre água e gás
- **Transporte de soluto** via advecção-difusão
- **Dissolução de metano** com base em uma formulação inspirada na Lei de Henry
- **Heterogeneidade do meio poroso** em `kx`, `kz` e `φ`
- **Regime cíclico de operação** com fases de injeção, repouso e produção

---

## 🧮 Metodologia Numérica

O simulador resolve um sistema acoplado de equações diferenciais parciais não lineares utilizando:

1. **CVFD (Control Volume Finite Difference)** para discretização espacial em malha *block-centered*;
2. **IMPES** (*Implicit Pressure, Explicit Saturation*) para o acoplamento pressão–saturação;
3. **IMPEC** (*Implicit Pressure, Explicit Saturation, Implicit Concentration*) para o transporte de metano dissolvido;
4. **Gauss-Seidel** com iteração de **Picard** para o sistema implícito da pressão;
5. **Critério CFL** para controle dinâmico do passo de tempo;
6. **Esquema upwind** nas mobilidades e no transporte advectivo.

---

## 🧱 Estrutura Geral do Código

O código inclui módulos/funções para:

- definição de parâmetros e malha;
- inicialização dos campos de pressão, saturação e concentração;
- cálculo de propriedades e mobilidades;
- transmissibilidades nas interfaces;
- solução da equação de pressão;
- atualização explícita da saturação;
- solução implícita da concentração;
- scheduler de operação cíclica;
- cálculo de inventário;
- exportação de resultados em `.csv`.

---

## 📊 Saídas do Simulador

Durante a execução, o simulador pode gerar arquivos `.csv` com:

- **históricos temporais** de cada caso;
- **perfis centrais** de pressão, saturação e concentração;
- **campos 2D** de:
  - pressão
  - saturação de gás
  - concentração dissolvida
- **tabela-resumo** com métricas finais dos casos simulados.

Exemplos de arquivos gerados:

- `historico_base.csv`
- `historico_sem_gravidade.csv`
- `historico_sem_dissolucao.csv`
- `campo_pressao_*.csv`
- `campo_sg_*.csv`
- `campo_c_*.csv`
- `resumo_casos.csv`

---

## 🧪 Casos de Estudo

O simulador foi estruturado para rodar múltiplos cenários, incluindo:

- **caso base**
- **sem gravidade**
- **sem dissolução**
- **baixa permeabilidade**
- **alta porosidade**

Esses casos permitem análises comparativas e estudos de sensibilidade voltados ao capítulo de resultados do TCC.

---

## 🚀 Como Compilar e Executar

### Pré-requisitos

- Compilador C++ com suporte a C++17
- `g++`, MinGW, Dev-C++ ou ambiente equivalente
- Python 3 com `pandas` e `matplotlib` para pós-processamento

### Compilação no terminal

Linux / Colab:

```bash
g++ -O2 -std=c++17 main.cpp -o simulador_ugs
./simulador_ugs
