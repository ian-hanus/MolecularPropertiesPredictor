# Designing Selective Kinase Inhibitors
## Initial Thoughts
I will be attempting to predict pKi of a compound given the SMILES representation of a molecule. After reviewing the data, I want to take two different approaches to the modeling and compare them to each other. The first approach will be based on the molecular descriptors available in RDKit, and will utilize a classic multilayer perceptron network. The second approach is a graph based convolutional neural network, utilizing the spatial relations contained in the SMILES molecular representation.

## Molecular Descriptor-Based MLP
The descriptor-based MLP process can be found in the following files, in the following order:
1. MLPDataExploration.ipynb
2. MLPDataProcessing.ipynb
3. MLPModelDevelopment.ipynb

## Atomic Structure GCN
The atomic-structure graph based convolutional network can be found in GCNTotal.ipynb.

## Model Comparison
Mean squared error is used as the evaluation metric in this case because of the context of the problem. If a pKi is off by a small amount, it could still allow a molecule to bind to a targeted protein, so this candidate should still be considered. The MSEs for the models are:

| Kinase | MLP (MSE) | GCN (MSE) |
|--------|-----------|-----------|
| JAK1   |    0.1902 |    1.0834 |
| JAK2   |    0.6394 |    0.8823 |
| JAK3   |    0.7134 |    1.6503 |
| TYK2   |    0.4824 |    0.7419 |

While this shows that the MLP consistently outperformed the GCN, this is not a particularly fair comparison to make for the overall potential of the models. I fully flushed out many of my ideas for the MLP and optimized it, whereas the GCN is pretty barebones due to time constraints. Instead of comparing their evaluation metrics, I'll delve a bit into the pros and cons of each.

Descriptor-based modeling is generally quite dependent on the avaialable descriptors for a molecule. This means that the model is relying on the existence or generation of outside information, some of which isn't even necessarily germane to the task at hand. This is why feature understanding and engineering is so important for the descriptor based model. The data processing and training of this model will also be much faster and less compute intensive if that is a consideration worth taking into account.

Graph convolutional networks on the other hand are able to comprehend a molecule at its atomic structure. These are once again reliant on feature information, in this case atom and bond features, but they understand the molecular structure and how connected atoms interact. This will add information that can simply not be conveyed in a molecular descriptor-based model.

Of my two models, I would deploy the MLP because of its current performance. However, in the future I would focus my efforts on building out my GCN and I suspect that with enough time its performance would quickly overtake that of the MLP.

## References
Gawriljuk, Victor & Zin, Phyo Phyo & Puhl, Ana & Zorn, Kimberley & Foil, Daniel & Lane, Thomas & Hurst, Brett & Almeida Tavella, Tatyana & Costa, Fabio & Lakshmanane, Premkumar & Bernatchez, Jean & Godoy, Andre & Oliva, Glaucius & Siqueira-Neto, Jair & Madrid, Peter & Ekins, Sean. (2021). Machine Learning Models Identify Inhibitors of SARS-CoV-2. Journal of Chemical Information and Modeling. 61. 10.1021/acs.jcim.1c00683.

N. Khuri and S. Deshmukh, "Machine Learning for Classification of Inhibitors of Hepatic Drug Transporters," 2018 17th IEEE International Conference on Machine Learning and Applications (ICMLA), Orlando, FL, USA, 2018, pp. 181-186, doi: 10.1109/ICMLA.2018.00034.

Wang, F., Chen, YT., Yang, JM. et al. A novel graph convolutional neural network for predicting interaction sites on protein kinase inhibitors in phosphorylation. Sci Rep 12, 229 (2022). https://doi.org/10.1038/s41598-021-04230-7

Wieder, Oliver & Kohlbacher, Stefan & Kuenemann, Mélaine & Garon, Arthur & Ducrot, Pierre & Seidel, Thomas & Langer, Thierry. (2020). A compact review of molecular property prediction with graph neural networks. Drug Discovery Today: Technologies. 37. 10.1016/j.ddtec.2020.11.009. 
