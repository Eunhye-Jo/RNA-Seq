# Welcome to Jupyter!

This repo contains an introduction to [Jupyter](https://jupyter.org) and [IPython](https://ipython.org).

Outline of some basics:

* [Notebook Basics](../examples/Notebook/Notebook%20Basics.ipynb)
* [IPython - beyond plain python](../examples/IPython%20Kernel/Beyond%20Plain%20Python.ipynb)
* [Markdown Cells](../examples/Notebook/Working%20With%20Markdown%20Cells.ipynb)
* [Rich Display System](../examples/IPython%20Kernel/Rich%20Output.ipynb)
* [Custom Display logic](../examples/IPython%20Kernel/Custom%20Display%20Logic.ipynb)
* [Running a Secure Public Notebook Server](../examples/Notebook/Running%20the%20Notebook%20Server.ipynb#Securing-the-notebook-server)
* [How Jupyter works](../examples/Notebook/Multiple%20Languages%2C%20Frontends.ipynb) to run code in different languages.

You can also get this tutorial and run it on your laptop:

    git clone https://github.com/ipython/ipython-in-depth

Install IPython and Jupyter:

with [conda](https://www.anaconda.com/download):

    conda install ipython jupyter

with pip:

    # first, always upgrade pip!
    pip install --upgrade pip
    pip install --upgrade ipython jupyter

Start the notebook in the tutorial directory:

    cd ipython-in-depth
    jupyter notebook


```python
import pandas as pd
source={'STEP': ["1","2","3","4","5"], 
'Contents':["quality check","trimming and adapter & bases with low quality","aligning RNA-seq reads to a reference","quantifying gene and isoform abundances, normalization","visualization with DEseq2, hclust, heatmap"], 
'Tools':["Fastqc","Trimmomatic", "STAR","RSEM","ggplot in R"],
'Output File': ["html","trimmed fastq","Genome-mapped bam Transcriptome-mapped bam","Expected count FPKM TPM","n"]}

DataA=pd.DataFrame(source)

DataA
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>STEP</th>
      <th>Contents</th>
      <th>Tools</th>
      <th>Output File</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>quality check</td>
      <td>Fastqc</td>
      <td>html</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>trimming and adapter &amp; bases with low quality</td>
      <td>Trimmomatic</td>
      <td>trimmed fastq</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>aligning RNA-seq reads to a reference</td>
      <td>STAR</td>
      <td>Genome-mapped bam Transcriptome-mapped bam</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>quantifying gene and isoform abundances, norma...</td>
      <td>RSEM</td>
      <td>Expected count FPKM TPM</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>visualization with DEseq2, hclust, heatmap</td>
      <td>ggplot in R</td>
      <td>n</td>
    </tr>
  </tbody>
</table>
</div>


