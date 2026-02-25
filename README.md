# Power Sum Method (PSM) - Análise de Fluxo de Potência

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.20%2B-013243.svg)](https://numpy.org/)
[![Pandas](https://img.shields.io/badge/Pandas-1.3%2B-150458.svg)](https://pandas.pydata.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.4%2B-11557c.svg)](https://matplotlib.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 📋 Descrição

Implementação computacional do **Método de Soma de Potências (Power Sum Method - PSM)** para análise de fluxo de potência em sistemas de distribuição radial de energia elétrica.

Este projeto apresenta um algoritmo iterativo eficiente para calcular:
- Fluxo de potência ativa (P) e reativa (Q) em cada trecho da rede
- Tensões nodais (U) em todas as barras do sistema
- Perdas técnicas de potência em cada segmento
- Perfis de tensão sob diferentes condições de carga
- Compensação reativa com bancos de capacitores

## 🎯 Características

- ✅ **Convergência Rápida**: Otimizado para sistemas radiais de distribuição
- ✅ **Análise Multi-Cenário**: Simulação de diferentes níveis de carga
- ✅ **Compensação Reativa**: Modelagem de bancos de capacitores
- ✅ **Visualizações**: Gráficos de perfis de tensão

## 🚀 Começando

### Pré-requisitos

```bash
Python 3.8 ou superior
```

### Instalação

1. Clone o repositório:
```bash
git clone https://github.com/Jeisianyf/power-sum-method.git
cd power-sum-method
```

2. Instale as dependências:
```bash
pip install numpy pandas matplotlib
```

### Uso Rápido

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# Carregar dados da rede
data = pd.read_csv('https://raw.githubusercontent.com/Jeisianyf/power-sum-method/main/dados.csv')

# Executar análise de fluxo de potência
from power_sum_method import sum_of_powers_method

# Análise com carga nominal (100%)
results = sum_of_powers_method(data, num_iter=10, load_factor=1.0)
P, Q, U, losses_P, losses_Q = results

# Visualizar perfil de tensão
plt.plot(U/U[0])
plt.xlabel('Barra')
plt.ylabel('Tensão (p.u.)')
plt.title('Perfil de Tensão do Alimentador')
plt.grid(True)
plt.show()
```

## 🔬 Metodologia

### Algoritmo PSM

O Método de Soma de Potências opera em três etapas iterativas:

1. **Varredura Reversa (Backward Sweep)**
   - Cálculo dos fluxos de potência das barras terminais para a subestação
   - Soma das potências instaladas e perdas em cada trecho

2. **Cálculo de Tensões (Voltage Calculation)**
   - Determinação das tensões nodais considerando quedas de tensão
   - Uso da equação de queda de tensão em sistemas de distribuição

3. **Atualização de Perdas (Loss Update)**
   - Recálculo das perdas técnicas com base nos novos valores de tensão
   - Iteração até convergência

### Equações Fundamentais

**Fluxo de Potência:**
```
P[i] = P_carga[i] + Σ P[j] + P_perdas[i]
Q[i] = Q_carga[i] + Σ Q[j] + Q_perdas[i]
```

**Perdas nos Trechos:**
```
P_perdas = R × (P² + Q²) / U²
Q_perdas = X × (P² + Q²) / U²
```

**Queda de Tensão:**
```
ΔU = (R×P + X×Q) / U
```

## 📈 Cenários de Análise

O notebook implementa três cenários de operação:

| Cenário | Fator de Carga | Descrição |
|---------|---------------|-----------|
| **Carga Leve** | 50% | Período de baixa demanda |
| **Carga Média** | 90% | Condição típica de operação |
| **Carga Pesada** | 120% | Sobrecarga ou pico de demanda |

### Critérios de Qualidade

- **Tensão Mínima Aceitável**: 0.93 p.u. (93% da tensão nominal)
- **Tensão Máxima Aceitável**: 1.05 p.u. (105% da tensão nominal)
- Conforme ANEEL PRODIST Módulo 8

## 🔋 Compensação Reativa

O projeto inclui análise de compensação reativa usando bancos de capacitores:

- **Localização Estratégica**: Próximo às barras com maiores quedas de tensão
- **Dimensionamento**: 300-600 kVAr por banco
- **Benefícios**:
  - Melhoria do perfil de tensão
  - Redução de perdas técnicas
  - Liberação de capacidade do sistema

## 📚 Estrutura dos Dados

O arquivo CSV deve conter as seguintes colunas:

| Coluna | Descrição | Unidade |
|--------|-----------|---------|
| `de` | Barra de origem do trecho | - |
| `para` | Barra de destino do trecho | - |
| `vi` | Tensão inicial | kV |
| `Sinst` | Potência instalada | MVA |
| `fp` | Fator de potência | - |
| `r` | Resistência por km | Ω/km |
| `x` | Reatância por km | Ω/km |
| `dist` | Comprimento do trecho | km |

### Normas Técnicas

- **ANEEL PRODIST** - Módulo 8: Qualidade de Energia Elétrica

## 🛠️ Tecnologias Utilizadas

- **Python 3.8+**: Linguagem de programação
- **NumPy**: Computação numérica e álgebra linear
- **Pandas**: Manipulação e análise de dados
- **Matplotlib**: Visualização de dados e gráficos
- **Jupyter Notebook**: Ambiente interativo de desenvolvimento

## 📝 Exemplos de Aplicação

### 1. Análise Básica de Fluxo de Potência

```python
from power_sum_method import sum_of_powers_method

# Carregar dados
data = pd.read_csv('dados.csv')

# Executar análise
P, Q, U, losses_P, losses_Q = sum_of_powers_method(data, num_iter=10, load_factor=1.0)

# Exibir resultados
print(f"Tensão mínima: {min(U/U[0]):.4f} p.u.")
print(f"Perdas totais: {sum(losses_P)/1000:.2f} kW")
```

### 2. Comparação de Cenários

```python
# Simular diferentes níveis de carga
scenarios = {'Leve': 0.5, 'Média': 0.9, 'Pesada': 1.2}
results = {}

for name, factor in scenarios.items():
    results[name] = sum_of_powers_method(data, 10, factor)

# Plotar comparação
for name, (P, Q, U, _, _) in results.items():
    plt.plot(U/U[0], label=name)
    
plt.legend()
plt.show()
```

### 3. Otimização de Bancos de Capacitores

```python
# Testar diferentes configurações de compensação
capacitor_sizes = [300e3, 450e3, 600e3]  # VAr

for size in capacitor_sizes:
    # Modificar função para incluir compensação
    results = sum_of_powers_method_with_compensation(data, 10, 1.2, size)
    # Analisar melhoria no perfil de tensão
```

## 📊 Resultados Esperados

Ao executar o notebook completo, você obterá:

- ✅ Perfis de tensão para diferentes cenários de carga
- ✅ Cálculo de perdas técnicas no sistema
- ✅ Análise comparativa entre cenários
- ✅ Efeito da compensação reativa no perfil de tensão
- ✅ Identificação de barras críticas
- ✅ Recomendações para melhoria do sistema
