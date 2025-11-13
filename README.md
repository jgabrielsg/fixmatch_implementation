# Implementação do FixMatch (Aprendizado Semi-Supervisionado)

Este repositório contém o trabalho de implementação e análise do algoritmo FixMatch para a disciplina de Deep Learning da Graduação

### Autores
- João Gabriel
- Guilherme Buss
- Vinícius Nascimento

### Objetivo

Este projeto implementa o FixMatch, um algoritmo de Aprendizado Semi-Supervisionado (SSL). O objetivo principal é demonstrar como um modelo de Deep Learning pode ser treinado para classificar imagens com alta acurácia, mesmo com uma quantidade extremamente limitada de dados rotulados, ao "aprender" com uma vasta quantidade de dados não rotulados.

### Metodologia
Nossa implementação, baseada no paper _FixMatch_, combina duas técnicas principais para ensinar o modelo a usar dados não rotulados:
- Pseudo-Rotulagem (Pseudo-Labeling): O modelo primeiro gera uma previsão para uma versão "fracamente" aumentada de uma imagem não rotulada. Se a confiança do modelo nessa previsão for muito alta (acima de um limiar $\tau$, ex: 95%), esse rótulo previsto é usado como um _pseudo-rótulo_ (um gabarito temporário).
- Regularização por Consistência (Consistency Regularization): O modelo é então forçado a prever esse mesmo pseudo-rótulo ao olhar para uma versão "fortemente" aumentada (muito distorcida) da mesma imagem.

A função de perda final é, daí, uma soma ponderada da perda supervisionada ($L_s$, calculada nos dados rotulados) e não supervisionada ($L_u$, calculada nos pseudo-rótulos confiáveis).

### Detalhes da Implementação:
- Dataset: CIFAR-10
- Modelo Base: ResNet-18 (treinada do zero)

Augmentations:
- Fraca: Flip horizontal e Crop aleatório.
- Forte: RandAugment (aplicado em cima da aumentação fraca).

### Experimentos e Sucessos

Para avaliar a eficácia do FixMatch, comparamos a sua acurácia contra um modelo "Baseline" (treinado apenas com os poucos dados rotulados). Rodamos os testes em quatro cenários:

- 1 rótulo por classe (Total: 10 rótulos supervisionados)

- 4 rótulos por classe (Total: 40 rótulos supervisionados)

- 25 rótulos por classe (Total: 250 rótulos supervisionados)

- 400 rótulos por classe (Total: 4.000 rótulos supervisionados)

(O restante das 50.000 imagens foi sempre usado como dados não rotulados).

### Resultados Principais

1. O FixMatch supera o Baseline (quando há um sinal mínimo):
- Nossos experimentos mostraram que, nos cenários com 25 e 400 rótulos por classe, o FixMatch foi superior ao modelo baseline, onde o algoritmo for eficaz em extrair informação útil das dezenas de milhares de imagens não rotuladas para melhorar a generalização e o desempenho final.

2. O Desafio com Pouquíssimos Rótulos:
- Com tão poucos rótulos, o modelo "professor" (que gera os pseudo-rótulos) não é confiável.
- Ele gera pseudo-rótulos errados com alta confiança, "sujando" o treinamento do modelo "aluno".
- Isso demonstra que o FixMatch precisa de uma supervisão inicial decente para funcionar, o que conseguimos a partir de 25 rótulos por classe.

---

### Recursos do Projeto

### Vídeo da Apresentação (12 min)
Link: https://www.youtube.com/watch?v=thtC3vh357Y

### Modelos Treinados (.pth) e Código Python
Link: https://drive.google.com/drive/folders/1qiU-mSov5AG79mYLk4Drf_YU2ZeGuxvv?usp=sharing

---
