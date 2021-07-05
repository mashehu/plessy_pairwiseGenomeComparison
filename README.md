# Pairwise Genome Alignment

## Mandatory parameters

 * `--target`: path to one genome file in FASTA format.  It will be indexed.

 * `--input`: path to a sample sheet in tab-separated format with one header
   line `id	file`, and one row per genome (ID and path to FASTA file).

   — or —

   `--query`: patch to one genome file in FASTA format.

## Options

 * `--seeding_scheme` selects the name of the [LAST seed](https://gitlab.com/mcfrith/last/-/blob/main/doc/last-seeds.rst)
   The default (`NEAR`) searches for “_short-and-strong (near-identical)
   similarities_ … _with many gaps (insertions and deletions)_”.  Among
   alternatives, there is `YASS` for “_long-and-weak similarities_” that
   “_allow for mismatches but not gaps_”.

 * `--lastal_args` defaults to `-E0.05 -C2` and is applied to both
   the calls to `last-train` and `lastal`, like in the
   [LAST cookbook](https://gitlab.com/mcfrith/last/-/blob/main/doc/last-cookbook.rst).

 * --skip_tantan will leave intact the original soft-masking of the query
   sequences instead of re-masking with `tantan`.

## Fixed arguments

 * `--revsym` is hardcoded the call to `last-train` as the DNA strands
   play equivalent roles in the studied genomes.

## Test

### test remote

    nextflow run oist/plessy_pairwiseGenomeComparison -r main -profile oist --input testInput.tsv --target https://raw.githubusercontent.com/nf-core/test-datasets/modules/data/genomics/sarscov2/illumina/fasta/contigs.fasta
    nextflow run oist/plessy_pairwiseGenomeComparison -r main -profile oist --query https://raw.githubusercontent.com/nf-core/test-datasets/modules/data/genomics/sarscov2/genome/genome.fasta --target https://raw.githubusercontent.com/nf-core/test-datasets/modules/data/genomics/sarscov2/illumina/fasta/contigs.fasta

### test locally

    nextflow run ./main.nf -profile oist --input testInput.tsv --target https://raw.githubusercontent.com/nf-core/test-datasets/modules/data/genomics/sarscov2/illumina/fasta/contigs.fasta
    nextflow run ./main.nf -profile oist --query https://raw.githubusercontent.com/nf-core/test-datasets/modules/data/genomics/sarscov2/genome/genome.fasta --target https://raw.githubusercontent.com/nf-core/test-datasets/modules/data/genomics/sarscov2/illumina/fasta/contigs.fasta


## Advanced use

### Override computation limits

Computation resources allocated to the processe are set with standard _nf-core_
labels in the [`nextflow.config`](./nextflow.config) file of the pipeline.  To
override their value, create a configuration file in your local directory and
add it to the run's configuration with the `-c` option.

For instance, with file called `overrideLabels.nf` containing the following:

```
process {
  withLabel:process_high {
    time = 3.d
  }
}
```

The command `nextflow -c overrideLabels.nf run …` would set the execution time
limit for the training and alignment (whose module declare the `process_high`
label) to 3 days instead of the 1 hour default.


## Semantic versioning

I will apply [semantic versioning](https://semver.org/) to this pipeline:

 - Major increment when the interface changes in a way that is
   backwards-incompatible, in the sense that a run with the same command and
   the same data would produce a different result (except for non-deterministic
   computations).

 - Minor increment for any other change of the interface, such as additions of
   new functionalities.

 - Patch increment for changes that do not modify the interface (bug fixes,
   minor software and module updates, documentation changes, etc.)
