# Airbnb-dataset - Cognitivo.ai  

- a. Como foi a definição da sua estratégia de modelagem?  
    - Optei pela `classificação` da variável dependente `room_type`
    - Examinei o conjunto de dados, variáveis e tipagem  
    - Separei as `features` por `tipo`  
        - posso por conveniência juntar estas listas  
    - Escolhi trilhar um caminho de `eliminação de variáveis` e ao longo fui aplicado técnicas estatísticas para este propósito  
        - Modelos com menos número de recursos têm maior explicabilidade  
        - É mais fácil implementar modelos de aprendizado de máquina com um menor número possível de features  
        - Menos features levam a uma generalização aprimorada que, por sua vez, reduz o over-training  
        - A seleção de features remove a redundância de dados  
        - O tempo de treinamento dos modelos com menos features é significativamente menor   
        - Modelos com menos features são menos propensos a erros     
    - Selecionei métodos de `input value` seguindo o método comumente aceito e utilizado para valores `NaN`  
        - `Mediana` para números reais    
        - `Mediana` com `arredondamento` para números inteiros    
        - O caracter `?` nas variáveis `categóricas`    
        - `Moda` nas variáveis `booleanas`  
    - Pelas variáveis categóricas e suas respectivas frequências e distribuições eu selecionei o método da `amostragem`:  
        - `Estratificada` pela variável dependente `room_type`  
            - O `desbalancemaneto` entre as `4 classes` é `alto`   
        - Transformei as `categorias` em `números ordinais`, pois assim possibilitará a utilização de diversos algoritmos baseados em cálculos e estimativas   
        - Apliquei `heuríticas` de `resample`  
            - Afim de avaliar o método que melhor `maximiza` `F1-Score` e `Balanced Accuracy`    
    - Apliquei `heurísticas` de eliminação de `outilers` com o intuito de diminuir `discrepância` entre valores máximos e mínimos e, com isso, de `melhorar` a convergência/ajuste dos algoritmos de classificação tendo cuidado para evitar o `under` e o `over training`   
        - Obs.: no final foi decidido `não` utilizar heurística de eliminação de outliers, pois o resultado sem este uso apresentou ótimos resultados  
            - Desse modo é preferível desconsiderar a remoção dos outiliers    
    - Selecionei a `função de custo`  
        - Neste momento eu aplico a função de custo no dataset original, ou seja, sem minimizar o desbalanceamento   
            - O intuito é `comparar` F1-Score e Balanced Accuracy da `função de custo` com os resultados da classificação destas métricas que utilizaram os datasets com as `heurísticas de desbalanceamento`. 
    

- b. Como foi definida a função de custo utilizada?  
    - Escolhi a Regressão Logística como função de custo  
        - Por ser um algoritmo de amplo uso e com aplicação em diversas áreas    
    - Utilizei a library `LazyPredict`  
        - Apliquei as heurísticas de desbalanceamento e   
        - E de uma só vez comparei vários algoritmos de classificação com parâmetros standard do LazyPredict   
            - Que inclui também o método utilizado na função de custo, só que desta vez sobre o dataset rebalanceado   
                - Com isso, foi possível a comparação com a função de custo sobre o dataset `original` e nos demais datasets     

- c. Qual foi o critério utilizado na seleção do modelo final?  
    - Maximização do F1-Score e  
    - Maximização do Balanced Accuracy  

- d. Qual foi o critério utilizado para validação do modelo? Por que escolheu utilizar este método?  
    - Critério: 
        - sklearn.model_selection.train_test_split  
            - Foi feito um cálculo de tamanho amostral, onde foram separados `X_train, ỳ_train` e `X_remaining, y_remaining`  
                - Onde, `X_remaining, y_remaining``X_remaining, y_remaining` foram utilizados no final como conjunto de `teste`  
            - Em `X_train, ỳ_train` foi separado `20%` para `X_valid, y_valid`  
                - Sendo estes últimos utilizados para treinamento e validação, respectivamente  
        - from sklearn.metrics import classification_report  
        - from sklearn.metrics import accuracy_score, balanced_accuracy_score  
        - from sklearn.metrics import confusion_matrix  
    - Foi escolhido este método por ser um procedimento muito usual e mensura muito bem o desempenho de qualquer algoritmo de classificação  

- e. Quais evidências você possui de que seu modelo é suficientemente bom?  
    - No final há um quadro onde são mostrados os resultados da função de custo `Regressão Logística` comparados com mais três abordagens utilizando o mesmo algoritmo
    
                            sklearn - LogisticRegression   Acc    B-Acc   F1 Score <- FUNÇÃO DE CUSTO <- ORIGINAL  
                                                          0.31   0.70     0.39  
          ------------- LazyPredict -------------------- 
          Sem resample              - LogisticRegression	0.89	 0.52	    0.89 <- FUNÇÃO DE CUSTO <- LazyPredict  
          Tomek                     - LogisticRegression	0.91	 0.51	    0.91  
          NeighbourhoodCleaningRule - LogisticRegression	0.99	 0.57	    0.99 <- MELHOR RESULTADO  
