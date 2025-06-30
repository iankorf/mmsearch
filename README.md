mmsearch
========

## Quick Start ##

The `mmsearch` program creates a sqlite3 database for rapid lookups among MMRRC
ids, MGI ids, gene symbols, GO terms, and MP terms.

Download files into a build directory (default is `./build`). This may take a
minute or two.

```
mmsearch download
```

Create the sqlite3 database. This takes about 1.5 minutes.

```
mmsearch create
```

Make 6k random queries across the database. This takes about a second.

```
mmsearch test
```

## Tables ##

The tables are created once by `mmsearch create` and never updated. All of the
tables are de-normalized and indexed for optimal retrieval. It is intended for
clients to connect to the `mmsearch.db` database in read-only mode.

### genes

```
CREATE TABLE genes(symbol PRIMARY KEY, mgi_id UNIQUE, desc);
```

| symbol | mgi_id      | desc
|:-------|:------------|:---------------------------------------------------
| Auts2  | MGI:1919847 | Autism susceptibility gene 2 protein homolog
| Cfap77 | MGI:2685669 | Cilia- and flagella-associated protein 77
| Redic1 | MGI:3613655 | Regulator of DNA class I crossover intermediates 1

### go_nodes

```
CREATE TABLE go_nodes(go_id PRIMARY KEY, label, desc);
```

| go_id      | label                     | desc
|:-----------|:--------------------------|:-----------------------------------
| GO_0000001 | mitochondrion inheritance | The distribution of mitochondria...
| GO_0000011 | vacuole inheritance       | The distribution of vacuoles...
| GO_0000016 | lactase activity          | Catalysis of the reaction...

### mp

```
CREATE TABLE mp(mp_id PRIMARY KEY, label, desc);
```

| mp_id      | label               | desc
|:-----------|:--------------------|:-----------------------------------------
| MP_0000001 | mammalian phenotype | the observable morphological...
| MP_0000017 | big ears            | outer ears of a greater than normal...
| MP_0000274 | enlarged heart      | increase over normal size of the heart...

### mgi2go

```
CREATE TABLE mgi2go(mgi_id, go_id);
CREATE INDEX idx1 ON mgi2go(mgi_id);
CREATE INDEX idx2 ON mgi2go(go_id);
```

| mgi_id      | go_id
|:------------|:----------
| MGI:1919847 | GO_0015629
| MGI:1919847 | GO_0005515
| MGI:2140937 | GO_0000166
| MGI:2140937 | GO_0003924

### mmrrc2mgi

```
CREATE TABLE mmrrc2mgi(mmrrc_id, mgi_id);
CREATE INDEX idx3 ON mmrrc2mgi(mmrrc_id);
CREATE INDEX idx4 ON mmrrc2mgi(mgi_id);
```

| mmrrc_id         | mgi_id
|:-----------------|:-----------
| MMRRC:000001-UNC | MGI:99501
| MMRRC:000001-UNC | MGI:1316726
| MMRRC:000002-UNC | MGI:109392

### mmrrc2mp

```
CREATE TABLE mmrrc2mp(mmrrc_id, mp_id);
CREATE INDEX idx5 ON mmrrc2mp(mmrrc_id);
CREATE INDEX idx6 ON mmrrc2mp(mp_id);
```

| mmrrc_id         | mp_id
|:-----------------|:----------
| MMRRC:000002-UNC | MP_0011747
| MMRRC:000011-UCD | MP_0000745
| MMRRC:000011-UCD | MP_0000774

### mmrrc2go

```
CREATE TABLE mmrrc2go(mmrrc_id, go_id);
CREATE INDEX idx9 ON mmrrc2go(mmrrc_id);
CREATE INDEX idx10 ON mmrrc2go(go_id);
```

| mmrrc_id         | go_id
|:-----------------|:----------
| MMRRC:000027-MU  | GO_0051926
| MMRRC:000028-MU  | GO_0005794
| MMRRC:000028-MU  | GO_0016020

### mmrrc2gene

```
CREATE TABLE mmrrc2gene(mmrrc_id, symbol);
CREATE INDEX idx7 ON mmrrc2gene(mmrrc_id);
CREATE INDEX idx8 ON mmrrc2gene(symbol);
```

| mmrrc_id         | symbol
|:-----------------|:------------
| MMRRC:000001-UNC | Fgg
| MMRRC:000001-UNC | Fga
| MMRRC:000002-UNC | Esr2
