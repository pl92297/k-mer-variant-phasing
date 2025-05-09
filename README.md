# Joint k-mer and variant phasing

[![DOI](https://zenodo.org/badge/636406459.svg)](https://zenodo.org/doi/10.5281/zenodo.10655504)

<!---

[![Actions Status](https://github.com/mrvollger/k-mer-variant-phasing/workflows/CI/badge.svg)](https://github.com/mrvollger/k-mer-variant-phasing/actions)
-->

This is a pipeline designed to phase variants and PacBio Hifi data using a combination of k-mers and variants.

However, by running it in different modes you can also use it to phase using only variants and not k-mers (hiphase), or to just generate variant calls without any phasing of reads or the vcf (DeepVariant).

## Install

Please start by installing [pixi](https://pixi.sh/latest/) which handles the environment of this Snakemake workflow. You also must have `apptainer` available in your path/system.

You can then install the `pixi` environment by cloning this repository and running:

```bash
pixi install
```

`pixi` then handles the execution of the Snakemake workflows.

We then recommend you run a partial test of your installation with:

```bash
pixi run test
```

## Usage

First set up a configuration file. See `config/config.yaml` for a commented example.
Then run snakemake with the following command pointing to your configuration file.

```
pixi run snakemake --profile profiles/slurm-executor --configfile config/your_config.yaml
```

## Running without parental data

You can run the pipeline the same way, it will only use `hiphase` instead of a combination of `hiphase` and k-mers to phase.
See `config/no_parental.yaml` for an example input file.

## Running only DeepVariant

First set up a configuration file. See `config/deepvariant.yaml` for a commented example.
Then run snakemake with the following command pointing to your configuration file.

```
pixi run snakemake deepvariant --profile profiles/slurm-executor --configfile config/your_config.yaml
```

## Running only alignment (pbmm2)

First set up a configuration file. See `config/align.yaml` for a commented example.
Then run snakemake with the following command pointing to your configuration file.

```
pixi run snakemake pbmm2 --profile profiles/slurm-executor --configfile config/your_config.yaml
```

## Running from another directory

If you want to run this Snakemake from another directory you can do so with:

```bash
pixi run --manifest-path /path/to/snakemake/pixi.toml snakemake ...
```

where you update `/path/to/snakemake/pixi.toml` to the path of the `pixi.toml` you cloned.

### Dependencies

Requirements for executing the pipeline are, apptainer/singulariy or docker and pixi.

If you wish to distribute jobs across a cluster you will need to install the appropriate [snakemake executor plugin](https://snakemake.github.io/snakemake-plugin-catalog/). For example, to use SLURM you can install the `snakemake-executor-slurm` plugin using pip:

```
pip install snakemake-executor-plugin-slurm
```

Additional dependencies are managed automatically by snakemake using mamba. For Stergachis lab members be sure to include the following in your `.bashrc` if you want to use a pre-computed conda env.

```
export SNAKEMAKE_CONDA_PREFIX=/mmfs1/gscratch/stergachislab/snakemake-conda-envs
```
