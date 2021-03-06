# DecagonPredictorDataSets

This repository holds the data necessary to correctly run the code in its sister repository, 
[DecagonPredictor](https://github.com/jrectorb/DecagonPredictor).  Note that, due to Github's
restriction on file size, some files here are stored in gzip format.  However, one may use the
script at [here](https://github.com/jrectorb/DecagonPredictor/blob/master/get-data.sh) to get this directory in a
usable, uncompressed format.  In the following we provide description of the data contained within
this directory.

## Directory Structure

Within the directory [CommonData](CommonData) can be found only one directory: 
[NetworkEdgeLists](CommonData/NetworkEdgeLists).  This directory contains csv files representing 
edge lists of various graphs used during prediction for Decagon.  Namely, there exists a file 
for a drug-drug interaction network, a drug-protein interaction network, and a protein-protein
interaction network.  Due to file size, the drug-drug interaction network is stored in gzip format.

The directory [ModelSpecificData](ModelSpecificData) contains matrices which have 
been pretrained with Decagon on different data sets, as well as the test edges which were used
in that training regime.  Namely, we provide data for two runs of Decagon.  In the first, the model
was trained using _all_ possible data.  In the second, the model was trained with all data besides 
a few select side effects fully masked.  


#### Masked Side Effects
| Stitch Id | Side Effect Name |
| --------- | ---------------- |
| C0003126  | Anosmia          |
| C0020456  | Hyperglycaemia   |
| C0027947  | Neutropenia      |
| C0026780  | Mumps            |
| C0009193  | Coccydynia       |
| C0038019  | Spondylosis      |

Recall that for link prediction in Decagon we use the learned tensor decomposition z_i D_r R D_r z_j^T.
Here, z_i \in R^d is an embedding of a node in the afore-mentioned interaction networks where d is the
size of the final layer of Decagon.  D_r \in R^{d \times d} is a diagonal feature importance matrix for 
side effect r.  Finally, R is a global interaction matrix, learned in particular by convolutions within
Decagon's GNN.  The sister repository [DecagonPredictor](https://github.com/jrectorb/DecagonPredictor)
uses these constructions to make predictions.  In light of this, each sub-directory within 
[ModelSpecificData](ModelSpecificData) contains three numpy files: `EmbeddingImportance.npz`, `GlobalRelations.npy`,
and `embeddings.npy`.  The first of these (`EmbeddingImportance.npz`) is a collection of all D_r matrices.  
The second of these (`GlobalRelations.npy`) is the matrix R.  The third `embeddings.npy` are the node embeddings
z_i.  Finally, within each sub-directory here is also contained a `TestEdges.csv` file which details the edges
of the test set for that specific training of Decagon.

