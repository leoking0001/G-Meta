## [NeurIPS 2020] G-Meta: Graph Meta Learning via Local Subgraphs
#### Kexin Huang, Marinka Zitnik

Prevailing methods for graphs require abundant label and edge information for learning. When data for a new task are scarce, meta-learning allows us to learn from prior experiences and form much-needed inductive biases for fast adaption to the new task. Here, we introduce G-Meta, a novel meta-learning approach for graphs. G-Meta uses local subgraphs to transfer subgraph-specific information and make the model learn the essential knowledge faster via meta gradients. G-Meta learns how to quickly adapt to a new task using only a handful of nodes or edges in the new task and does so by learning from data points in other graphs or related, albeit disjoint label sets. G-Meta is theoretically justified as we show that the evidence for a particular prediction can be found in the local subgraph surrounding the target node or edge. Experiments on seven datasets and nine baseline methods show that G-Meta can considerably outperform existing methods by up to 16.3%. Unlike previous methods, G-Meta can successfully learn in challenging, few-shot learning settings that require generalization to completely new graphs and never-before-seen labels. Finally, G-Meta scales to large graphs, which we demonstrate on our new Tree-of-Life dataset comprising of 1,840 graphs, a two-orders of magnitude increase in the number of graphs used in prior work.

## Installation

Dependencies:
```
pytorch, dgl, python 3.7, numpy, networkx, scipy, tqdm, sklearn, pandas
```

## Run
To use 
```bash
cd G-Meta
# Single graph disjoint label, node classification (e.g. arxiv-ogbn)
python train.py --data_dir DATA_PATH --task_setup Disjoint
# Multiple graph shared label, node classification (e.g. Tissue-PPI)
python train.py --data_dir DATA_PATH --task_setup Shared
# Multiple graph disjoint label, node classification (e.g. Fold-PPI)
python train.py --data_dir DATA_PATH --task_setup Disjoint
# Multiple graph shared label, link prediction (e.g. FirstMM-DB)
python train.py --data_dir DATA_PATH --task_setup Shared --link_pred_mode True
```

It also supports various parameters input:

```bash
python train.py --data_dir # str: data path
				--task_setup # 'Disjoint' or 'Shared': task setup, disjoint label or shared label
				--link_pred_mode # 'True' or 'False': link prediction or node classification
				--batchsz # int: number of tasks in total
				--epoch # int: epoch size
				--h # 1 or 2 or 3: use h-hops neighbor as the subgraph.
				--hidden_dim # int: hidden dim size of GNN
				--input_dim # int: input dim size of GNN
				--k_qry # int: number of query shots for each task
				--k_spt # int: number of support shots for each task
				--n_way # int: number of ways (size of the label set)
				--meta_lr # float: outer loop learning rate
				--update_lr # float: inner loop learning rate
				--update_step # int: inner loop update steps during training
				--update_step_test # int: inner loop update steps during finetuning
				--task_num # int: number of tasks for each meta-set
				--sample_nodes # int: when subgraph size is above this threshold, it samples this number of nodes from the subgraph
				--task_mode # 'True' or 'False': this is specifically for Tissue-PPI, where there are 10 tasks to evaluate.
				--num_worker # int: number of workers to process the dataloader. default 0.
				--train_result_report_steps # int: number to print the training accuracy.
				--val_result_report_steps # int: number of steps to report the validation accuracy, recommend to the batchsz/n.
```

To reproduce results, using the following code as example after you download the dataset from the section below.

arxiv-ogbn:
<details>
<summary>CLICK HERE FOR THE CODES!</summary>

```
python 
```
</details>

Tissue-PPI:
<details>
<summary>CLICK HERE FOR THE CODES!</summary>

```
python 
```
</details>

Fold-PPI:
<details>
<summary>CLICK HERE FOR THE CODES!</summary>

```
python 
```
</details>

FirstMM-DB:
<details>
<summary>CLICK HERE FOR THE CODES!</summary>

```
python 
```
</details>

Tree-of-Life:
<details>
<summary>CLICK HERE FOR THE CODES!</summary>

```
python 
```
</details>


## Data Processing

We provide the processed data files in the google drive link [here]().

To create your own dataset, you should create the following files and put it under the name below:

- `graph_dgl.pkl`: A list of DGL graph objects. For single graph G, just use [G].
- `features.npy`: An array of arrays [feat_1, feat_2, ...] where feat_i is the feature matrix of graph i. 

Then, for node classification, include the following files:
- `train.csv`, `val.csv`, and `test.csv`: Each file has two columns, the first one is 'X_Y' (node Y from graph X) and its label 'Z'. Each file corresponds to the meta-train, meta-val, meta-test set. Our 
- `label.pkl`: A dictionary of labels where {'X_Y': Z} means the node Y in graph X has label Z.  

For link prediction, note that the support set should contain only edges in the highly incomplete graph (e.g. 30% of links) whereas the query set node pairs should be in the rest 70% of links. That is in GNN message passing, it should ONLY pass on support set graph, otherwise, the query set performance would be biased. This is why we split the meta-train/val/test csv files into separate support and query files. Thus, for link prediction, include the following files instead:
- `train_spt.csv`, `val_spt.csv`, and `test_spt.csv`: Two columns, first one is 'A_B_C' (node B and C from graph A) and the second one is the label. This is for the node pairs in the support set, i.e. positive links should be in the underlying GNN graph.
- `train_qry.csv`, `val_qry.csv`, and `test_qry.csv`:Two columns, first one is 'A_B_C' (node B and C from graph A) and the second one is the label. This is for the node pairs in the query set, i.e. positive links should NOT be in the underlying GNN graph.
- `train.csv`, `val.csv`, and `test.csv`: Merge the above two csv files.
- `label.pkl`: A dictionary of labels where {'A_B_C': D} means the node B and node C in graph A has link status D. D can be 0 or 1 means no link or has link.

We also provide a sample data processing script in the data_process folder. See `node_process.py` and `link_process.py`.

## Cite us
```
@article{gmeta,
  title={Graph Meta Learning via Local Subgraphs},
  author={Huang, Kexin and Zitnik, Marinka},
  journal={NeurIPS},
  year={2020}
}
```

## Contact
Open an issue or send an email to kexinhuang@hsph.harvard.edu if you have any question. 

