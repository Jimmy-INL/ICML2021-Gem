## Setup environment
```sh
# pytorch 1.3.0 only support python 3.6
virtualenv --python=python3.6 gem
source gem/bin/activate
pip install -r gnnexp/requirements.txt
```

## Train classification models

```sh
cd gnnexp
python gnnexp/train.py --dataset=syn1
python gnnexp/train.py --dataset=syn4
python gnnexp/train.py --bmname=Mutagenicity
python gnnexp/train.py --bmname=NCI1
```
or you can directly use the checkpoint
```sh
unzip ckpt.zip
```

## Generate explaination of classification models by GNNExplainer
```sh
python gnnexp/explainer_main.py --dataset=syn1 --logdir=explanation/gnnexp
python gnnexp/explainer_main.py --dataset=syn4 --logdir=explanation/gnnexp
python gnnexp/explainer_main.py --dataset=Mutagenicity --graph-mode --logdir=explanation/gnnexp
python gnnexp/explainer_main.py --dataset=NCI1 --graph-mode --logdir=explanation/gnnexp
```
or you can directly unzip the explanation by
```sh
unzip gnnexp_explanation.zip
```

## Distillation
```sh
python generate_ground_truth.py --dataset=syn1 --top_k=6
python generate_ground_truth.py --dataset=syn4 --top_k=6
python generate_ground_truth_graph_classification.py --dataset=Mutagenicity --output=mutag --graph-mode --top_k=20
python generate_ground_truth_graph_classification.py --dataset=NCI1 --output=nci1_dc --graph-mode --top_k=20 --disconnected
```
or
```
unzip distillation.zip
```

## Train Gem
```sh
python explainer_gae.py --dataset=syn1 --distillation=syn1_top6 --output=syn1_top6
python explainer_gae.py --dataset=syn4 --distillation=syn4_top6 --output=syn4_top6
python explainer_gae_graph.py --distillation=mutag_top20 --output=mutag_top20 --dataset=Mutagenicity --gpu -b 128 --weighted --gae3 --loss=mse --early_stop --graph_labeling --train_on_positive_label --epochs=300 --lr=0.01
python explainer_gae_graph.py --distillation=nci1_dc_top20 --output=nci1_dc_top20 --dataset=NCI1 --gpu -b 128 --weighted --gae3 --loss=mse --early_stop --graph_labeling --train_on_positive_label --epochs=300 --lr=0.01
```

## Evaluate GNNExplainer and Gem
```sh
python test_explained_adj.py --dataset=syn1 --distillation=syn1_top6 --exp_out=syn1_top6 --top_k=6
python test_explained_adj.py --dataset=syn4 --distillation=syn4_top6 --exp_out=syn4_top6 --top_k=6
python test_explained_adj_graph.py --graph-mode --dataset=Mutagenicity --exp_out=mutag_top20 --distillation=mutag_top20 --top_k=15 --test_out=mutag_top20_top15
python test_explained_adj_graph.py --graph-mode --dataset=NCI1 --exp_out=nci1_dc_top20 --distillation=nci1_dc_top20 --top_k=15 --test_out=nci1_dc_top20_top15
```


## Visualization
Run `*.ipynb` files in Jupyter Notebook or Jupyter Lab.
