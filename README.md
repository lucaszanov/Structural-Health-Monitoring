# Structural-Health-Monitoring
Este repositório é destinado a trabalhos desenvolvidos com o intuito de aprendizado e divulgação de conhecimento em SHM (Structural Health Monitoring - Monitoramento de Integridade Estrutural). Os trabalhos são elaborados com base nos Homeworks da disciplina Introduction to SHM, ministrada pelo Prof. Dr. Samuel da Silva (https://www.samueldasilva.org/teaching/shm).

# Homework 1 - Statistical Classification using Mahalanobis Distance

Neste estudo, iremos analisar um sistema com 1 grau de liberdade (degree of freedom - DOF) composto por um elemento de inércia (massa - **m**) de valor 1
kg e um elemento de rigidez (mola - **k**) linear de valor 1000 N/m. O sistema é representado abaixo:

<p align="center">
<img src="https://github.com/lucaszanov/Structural-Health-Monitoring/blob/main/Homework_1/system-1DOF.png" width="280" height="200">
</p>
  
A ideia é simular um dano na estrutura através da perda de rigidez, isto é, progressivamente diminui-se o valor de k. Os dados em condição saudável (com k
= 1000 N/m) e em situação com dano (com k menor do que 1000 N/m) são utilizados então em um processo de classificação, através do aprendizado com
os dados em condição saudável e métricas de quantificação da qualidade da classificação utilizando dados de teste (tanto em condição saudável quanto em
condição com dano). Esse processo tem por objetivo verificar quais features são melhores para se utilizar na detecção do dano para este caso e também a
partir de qual porcentagem de perda de rigidez é possível detectar a presença do dano na dinâmica estrutural.

Os sinais que serão utilizados são obtidos via simulação numérica dada uma entrada do tipo sinal aleatório com 8192 amostras. Para cada condição, 100
testes foram feitos para garantir o elemento de aleatoriedade também no processo de aprendizado.

As features utilizadas neste estudo são:

1) Variance do sinal no domínio do tempo;
2) Kurtosis do sinal no domínio do tempo;
3) 2-Norm do sinal no domínio do tempo;
4) Índice <img src="https://render.githubusercontent.com/render/math?math=\gamma">, que é a razão da variância do sinal com dano/saudável(teste) e a variância do sinal de referência (saudável);
5) Natural frequency - via método de Welch;
6) RMSE do sinal no domínio do tempo;
7) Índice <img src="https://render.githubusercontent.com/render/math?math=\gamma_{AR}">, que, dado um modelo autoregressivo (AR) pré-determinado via sinal de referência, o índice é a razão entre a variância do erro de
predição do sinal com dano/saudável(teste) e a variância do erro de predição do sinal de referência (saudável).

Para o processo de classificação, foi utilizada a Distância de Mahalanobis, com features diferentes em cada parte do estudo. Além disso, para a definição do
threshold, são utilizados o máximo valor da referência (situação saudável) via `np.max()` , chi-square e IQR (Interquartile Range). Para o processo de
aprendizado (treino) são utilizadas 50% das amostras em condição saudável enquanto os 50% restantes são usadas para validação.
Dividiremos o estudo em 3 partes:

**A)** Perda gradual de rigidez até 65% do valor total (original) com um incremento de -1%. São consideradas como features as de número 1) a 7) (todas) e as
com melhores características para detecção são efetivamente utilizadas no processo. São levados em consideração na Distância de Mahalanobis a condição
saudável e somente 5 condições com dano na construção da curva ROC e matrizes de confusão, sendo estas com as perdas de 1%, 10%, 20%, 30% e 34%.

**B)** Perda gradual de rigidez até 80% do valor total (original) com um incremento de -0.5%. São consideradas como features as de número 1) a 6) e as com
melhores características para detecção são efetivamente utilizadas no processo. São levados em consideração na Distância de Mahalanobis a condição
saudável e todas as condições com dano.

**C)** Perda gradual de rigidez até 80% do valor total (original) com um incremento de -0.5%. São consideradas como features as de número 1) a 7) (todas) e as
com melhores características para detecção são efetivamente utilizadas no processo. São levados em consideração na Distância de Mahalanobis a condição
saudável e todas as condições com dano.
