## networkScience
引入資料集(python)操作：
```
import pandas as pd
import networkx as nx
import numpy as np
import matplotlib.pyplot as plt
import gzip

# List of datasets with attributes
datasets = pd.DataFrame([
    ('Facebook Northwestern University', '', './socfb-Northwestern25/socfb-Northwestern25.edges.gz'),
    ('IMDB movies and actors', '', './imdb/actors_movies.edges.gz'),
    ('IMDB actors costar', 'W', './imdb/actors_costar.edges.gz'),
    ('Twitter US politics', 'DW', './icwsm_polarization/retweet-digraph.edges.gz'),
    ('Enron Email', 'DW', './email-Enron/email-Enron.edges.gz'),
    ('Enron Executive Email', '', './ia-enron-only/ia-enron-only.edges'),
    ('Wikipedia math', 'D', './enwiki_math/enwiki_math.edges.gz'),
    ('Internet routers', '', './tech-RL-caida/tech-RL-caida.edges.gz'),
    ('US air transportation', '', './openflights/openflights_usa.edges.gz'),
    ('World air transportation', '', './openflights/openflights_world.edges.gz'),
    ('Yeast protein interactions', '', './bio-yeast-protein-inter/bio-yeast-protein-inter.edges'),
    ('C. elegans brain', 'DW', './celegansneural/celegansneural.edges'),
    ('Everglades ecological food web', 'DW', './eco-everglades/eco-everglades.edges'),
], columns=['Name', 'Type', 'File'])

df = datasets.set_index('Name')

# Iterate over each dataset
results = []
for idx, row in df.iterrows():
    fname = row['File']
    print(f"Processing {idx}...")
    
    if 'graphml' in fname:
        G = nx.read_graphml(fname)
    else:
        graph_class = nx.DiGraph() if 'D' in row['Type'] else nx.Graph()
        data_spec = [('weight', float)] if 'W' in row['Type'] else False
        G = nx.read_edgelist(fname, create_using=graph_class, data=data_spec)
    
    # Check if the graph is a multigraph
    if G.is_multigraph():
        MG = G
        G = nx.DiGraph() if MG.is_directed() else nx.Graph()
        G.add_edges_from((u,v) for u,v,i in MG.edges)

```