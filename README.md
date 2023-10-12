This fork fixes the issues on working with newer versions of polars (>= 0.17) due to deprecated method names compared to the [original branch](https://github.com/openvax/gtfparse).

Before the this fix is merged to the [origin](https://github.com/openvax/gtfparse), feel free to use this fixed version if [original gtfparse](https://github.com/openvax/gtfparse) is not working in your environment:

```bash
pip install 'gtfparse @ git+https://github.com/MingkaiChen/gtfparse'
```

Please also note that you may also need to install [pandas](https://pandas.pydata.org/) to get the appropriate part of gtfparse to work.

---

[![Build Status](https://travis-ci.org/openvax/gtfparse.svg?branch=master)](https://travis-ci.org/openvax/gtfparse) [![Coverage Status](https://coveralls.io/repos/openvax/gtfparse/badge.svg?branch=master&service=github)](https://coveralls.io/github/openvax/gtfparse?branch=master)
<a href="https://pypi.python.org/pypi/gtfparse/">
    <img src="https://img.shields.io/pypi/v/gtfparse.svg?maxAge=1000" alt="PyPI" />
</a>

gtfparse
========
Parsing tools for GTF (gene transfer format) files.

# Example usage

## Parsing all rows of a GTF file into a Pandas DataFrame

```python
from gtfparse import read_gtf

# returns GTF with essential columns such as "feature", "seqname", "start", "end"
# alongside the names of any optional keys which appeared in the attribute column
df = read_gtf("gene_annotations.gtf")

# filter DataFrame to gene entries on chrY
df_genes = df[df["feature"] == "gene"]
df_genes_chrY = df_genes[df_genes["seqname"] == "Y"]
```


## Getting gene FPKM values from a StringTie GTF file

```python
from gtfparse import read_gtf

df = read_gtf(
    "Transcripts.gtf",
    column_converters={"FPKM": float})

gene_fpkms = {
    gene_name: fpkm
    for (gene_name, fpkm, feature)
    in zip(df["seqname"], df["FPKM"], df["feature"])
    if feature == "gene"
}
```


