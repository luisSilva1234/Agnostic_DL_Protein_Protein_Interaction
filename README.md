# Using a Novel Unbiased Dataset and Deep Learning Model to Predict Protein-Protein Interactions
Proteins are indispensable to the living organisms, forming the backbone of almost all the cellular processes. However, these macromolecules rarely act alone, integrating the Protein-Protein Interaction (PPI). It should come as no surprise that their deregulation is one of the main causes to several disease states. The sudden surge of interest in this field of study motivated the development of innovative in silico methods. Nonetheless the obvious advances in recent years, the effectiveness of these computational methods remains questionable. There still are not enough evidences that support the use of just in silico techniques to predict PPIs not yet experimentally determined. It is proved that one of the primary reasons leading to this situation is the non-existence of a "gold-standard" negative interactions dataset. Contrary to the high abundance of publicly available positive interactions, the negative examples are often artificially generated, culminating in biased samples. In this paper we propose a new unbiased technique to create negative examples, that does not overly constraint the negative interactions distribution. Beyond the novel dataset creation task we also established a Deep Learning (DL) model as a tool to predict whether two individual proteins are capable of interacting with each other, using exclusively the complete raw amino acid sequences. The obtained results firmly indicate that the proposed model is a valuable tool to predict PPIs, while also highlighting that there is still some room for improvement when implemented in unbiased datasets.

## **Datasets** 
- **Independent dataset**: All the human multi-validated physical positive PPIs available in BioGRID were selected. For the negative interactions it was necessary to identify the unique proteins from the multi-validated positive interactions and put them in the set S of proteins. Afterwards, two distinct proteins were randomly sampled from S. Finally, the pair sampled was compared with the multi-validated and not multi-validated positive interactions and if it was not already labeled as one of them, then it was considered a new negative interaction.



- **Pan et al. dataset**: exclusively composed byhuman PPIs, whose positive interactions were obtained from the Human Protein References Databaseeradas. Afterwards the negative interactions were computationally generatedby the non co-localization method, where two randomly paired proteins found on distinct sub-cellular compartments are considered as non-interacting
- **Du et al. dataset**: A saccharomyces cerevisiae PPIs dataset was gathered from the Database of Interacting Proteins. The selection of negative interactions was also by the
non co-localization method




### **Network architecture:** 
Ultimamente foquei-me apenas numa arquitectura, a mais convencional, que foi aquela que para os testes anteriores tinha apresentado melhores resultados.

![image](https://user-images.githubusercontent.com/58522514/78458093-c5004780-76a6-11ea-9d83-225c6f55310e.png)

### **Hyper-tuning:**
Centrei a aminha abordagem no ajuste de alguns parâmetros, passando desde já a enumerá-los:
- número de filtros das camadas de convolução, escolhendo combinações aleatórias de 3 dos seguintes valores [64,96,128,160,192,224,256,512];
- tamanho do filtro, variando entre 2 e 3;
- tamanho do filtro de pooling, variando entre 2 e 3;
- número de neurónios das camadas fully connected, escolhendo combinações aleatórias de 3 dos seguintes valores [64,128,256,512,1024];
- drop rate das camadas de dropout que foram intercaladas com as camadas fully connected, sendo o seu valor 0.1, 0.2 ou 0.3;
- função de ativação, optando por 'relu' ou 'swish';
- otimizadores do modelo, optando por Adam, SGD, ou ainda RMSprop, assim como o learning Rate destes mesmo otimizadores, escolhendo entre 0.01 e 0.001.

### **Results:** 
Dividindo para cada um dos datasets:
- **Dataset1**: Uma vez que existem diversos artigos que optam por aplicar o mesmo dataset, optei por construir uma tabela onde se pode observar os resultados dos diferentes modelos propostos. Para este dataset o melhor modelo apresenta as seguintes características:
  - número de filtros das camadas de convolução:[96,128,256];
  - tamanho do filtro: 2;
  - tamanho do filtro de pooling: 2;
  - número de neurónios das camadas fully connected, [64,256,1024];
  - drop rate das camadas de dropout: 0.1;
  - função de ativação: 'relu';
  - otimizadores do modelo: Adam
  - learning Rate: 0.001.
  
Model  | Accuracy (%)| 
----------- | -------------
https://pubs.acs.org/doi/abs/10.1021/pr100618t | 97.9
https://bmcbioinformatics.biomedcentral.com/track/pdf/10.1186/s12859-017-1700-2 | 96.8
https://www.sciencedirect.com/science/article/abs/pii/S0025556418307168 | 98.3
Meu modelo  | 98.4


- **Dataset2**: Para este dataset o melhor modelo permitiu obter uma accuracy de 67% apresenta as seguintes características:
  - número de filtros das camadas de convolução:[64,224,128];
  - tamanho do filtro: 2;
  - tamanho do filtro de pooling: 2;
  - número de neurónios das camadas fully connected, [256,64,128];
  - drop rate das camadas de dropout: 0.1;
  - função de ativação: 'relu';
  - otimizadores do modelo: Adam
  - learning Rate: 0.001.

### **Future work:** 
De certa forma este trabalho de comparação, utilizando datasets já publicados, penso que deu para validar o modelo e chegar à conclusão que para obter resultados na gama do excelente, o dataset é fundamental. Futuramente tinha pensado em explorar implementar:
- dataset de dois benchmarks (https://pubs.acs.org/doi/abs/10.1021/acs.jcim.7b00028 e https://www.ncbi.nlm.nih.gov/pubmed/25657331) e avaliar os seus resultados para validar o modelo;
- dataset criado por mim onde apenas são consideradas como positivas as interações físicas multi-validated do Biogrid, sendo as interações negativas criadas pelo mesmo processo, já explicado anteriormente.
- avaliar a eficiência e capacidade de decisão de 3 diferentes arquitecturas:
  - arquitetura baseada na implementação de camadas convolucionais seguidas de uma camada LSTM que funcionam como extrator de features de cada proteína, sendo posteriormente concatenadas num vetor de features, específico para cada interação e avaliados e classificados como um par de interação ou não;
  - arquitetura que engloba diversas maneiras de representação de cada uma das proteínas, onde por exemplo cada protéina é representada por diferentes descritores, como conjoint triads, auto-covariance, entre outros. Sendo o principal foco avaliar se as diferentes informações provenientes das diferentes representações se podem complementar e com isso criar um modelo com melhor capacidade de generalização;
  - arquitetura onde apenas são utilizadas camadas convolucionais, onde o downsampling, tipicamente feito pelas camadas de pooling, 
é agora atingido pela utilização de diferentes valores de stride.

