# metagenome simulator

[![Build Status](https://travis-ci.org/fmaguire/metagenome_simulator.svg?branch=master)](https://travis-ci.org/fmaguire/metagenome_simulator)

Simple application to generate synthetic labelled metagenomes using ART and SeqAN.


## Installation

### Dependencies

- ART read simulator available 
[here](https://www.niehs.nih.gov/research/resources/software/biostatistics/art/index.cfm) 
or via conda (`conda install -c bioconda art`)

- SeqAN C++ library available [here](http://packages.seqan.de/) or via your package manager.

- CMake available [here](https://cmake.org/) or via your package manager.

- To run unit tests: googletest C++ library available [here](https://github.com/google/googletest)
or via your package manager.

### Build

Build uses the `cmake` configuration tool for out-of-source builds.

To build:

- `cd build`

- `cmake ..` if your seqan library is in an unusual location or cmake if missing
the module to find it you made need to specify the path to the library include
directory e.g. if it is in `~/conda/envs/metagenome_simulator/include`
`cmake -DSEQAN_INCLUDE_DIRS=~/conda/envs/metagenome_simulator/include ..`

- `make`

### Unit Tests

To build with unit tests you need the googletest library and
to run cmake with a `-Dtest=on` flag.  

As with SeqAN, if the googletest library is installed in an unusual location 
may need to specify
the path to the googletest library using the `-DGTEST_INCLUDE_DIRS=` option.

To run the unit tests:

`./bin/run_unit_tests`

## Usage

`metagenome_simulator --help` for detailed usage.

`metagenome_simulator [options] path_tsv_specifying_inputs`

### Input TSV

To run `metagenome_simulator` a TSV file must be provided that lists
the details of the input genomes, RGI (v4+) annotation TSV files and the
copy number for that genome.  The copy number is specified in integers and 
can be used to simulate relative abundance of source genomes
for the synthetic metagenome.

This file needs to be in the format of:

`path_to_genome_fasta \t path_to_annotation_tsv_for_genome \t genome_copy_number`

### Outputs

On successful completed run this tool will generate the following outputs:

- `output_synthetic_metagenome.fasta` - the synthetic 'assembled' metagenome
from the input genome contigs copied to specified numbers.

- `output.fq` the full set of simulated reads with specified sequencing 
error profile, fold coverage and read length.

- `output.sam` the sam file specifying where the simulated reads are sampled
from in the original genomes.

- `output_errFree.sam` the same sam file but containing reads free of simulated
sequening error.

- `output_labels.tsv` the output labels in the format of a TSV file with the 
columns: `read name`, `contig name`, `CARD ARO`, `CARD name`, `RGI cut-off`,
`overlap length`.  Where there is no overlap between the read and an annotation
these will all be `na`.

Finally, if the `clean` option is enabled, two additional files will be generated
only containing data from reads that are labelled:

- `output_clean.fq` only those reads that are labelled with an RGI annotation.

- `output_clean_labels.tsv` only the labels associated with those reads.

## Docker

First either pull the pre-built container from dockerhub:

`docker pull finlaymaguire/metagenome_simulator`

Or alternatively build yourself:

`git clone https://github.com/fmaguire/metagenome_simulator`
`docker build -t finlaymaguire/metagenome_simulator metagenome_simulator`

Then you can mount a local directory and run interactively within the container e.g. to mount a local directory called `metagenome_simulator_data` in your current directory to /data within the container and then start a bash shell in the container:

`docker run -i -v $PWD/metagenome_simulator_data:/data -t --entrypoint /bin/bash finlaymaguire/metagenome_simulator`
