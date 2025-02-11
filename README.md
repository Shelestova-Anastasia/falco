# Falco: FastQC Alternative Code

[![DOI](https://zenodo.org/badge/214499063.svg)](https://zenodo.org/badge/latestdoi/214499063)
[![Install on conda](https://anaconda.org/bioconda/falco/badges/platforms.svg)](https://anaconda.org/bioconda/falco)
[![Install on conda](https://anaconda.org/bioconda/falco/badges/license.svg)](https://anaconda.org/bioconda/falco)
[![Install on conda](https://anaconda.org/bioconda/falco/badges/downloads.svg)](https://anaconda.org/bioconda/falco)



This program is an emulation of the popular
[FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc) software to
check large sequencing reads for common problems.

Installing falco
================

## Installing through conda
If you use [anaconda](https://anaconda.org) to manage your packages, and the `conda` binary
is in your path, you can install the most recent release of `falco` by running
```
$ conda install -c bioconda falco
```

`falco` can be found inside the `bin` directory of your anaconda
installer.

## Installing from source (code release)

Compilation from source can be done by downloading a `falco` release from the
[releases](https://github.com/smithlabcode/falco/releases)
section above. Upon downloading the release (in `.tar.gz` or `.zip` format),
and inflating the downloaded file to a directory (e.g. `falco`), move to the
target directory the file was inflated (e.g. `cd falco`). You should see a
`configure` file in it. In this directory, run

```
$ ./configure CXXFLAGS="-O3 -Wall"
$ make all
$ make install
```
if you wish to install the falco binaries on a specific directory, you can use
the `--prefix` argument when running `./configure`, for instance:

```
$ ./configure CXXFLAGS="-O3 -Wall" --prefix=/path/to/installation/directory
```

The `falco` binary will be found in the `bin` directory inside the
specified prefix.

## Installing from a cloned repository
We strongly recommend using `falco` through stable releases as described above,
as the latest commits might contain undocumented bugs. For the more
advanced users who wish to test the most recent code, `falco` can be
installed by first cloning the repository

```
$ git clone https://github.com/smithlabcode/falco.git
$ cd falco
```

Once inside the generated repsotory directory, run
```
$ make all
$ make install
```

This should create a `bin` directory inside the cloned repository
containing `falco`.

### Required C++ dependencies

[zlib](https://zlib.net) is required to read gzip compressed FASTQ files. It is
usually installed by default in most UNIX computers and is part of the htslib
setup, but it can also be installed with standard package managers like
apt, brew or conda.

On Ubuntu, zlib C++ libraries can be installed with `apt`:
```
$ sudo apt install zlib1g zlib1g-dev
```

### Optional C++ dependencies

[htslib](https://github.com/samtools/htslib) is required to process bam
files. If not provided, bam files will be treated as unrecognized file
formats.

If htslib is installed, falco can be compiled with it by simply replacing the
configure command above with the `--enable-hts` flag:

```
$ ./configure CXXFLAGS="-O3 -Wall" --enable-hts
```

If `falco` was cloned from the repository, run the following commands
to allow BAM file reading:

```
$ make HAVE_HTSLIB=1 all
$ make HAVE_HTSLIB=1 install
```

If successfully compiled, `falco` can be used in BAM files the same way as it is
used with fastq and sam files.

Running falco
=============

Run falco in with the following command, where the `example.fq` file
provided can be replaced with the path to any FASTQ file you want to run
`falco`
```
$ falco example.fq
```

This will generate three files in the same directory as the input fastq file:
 * ``fastqc_data.txt`` is a text file with a summary of the QC
   metrics
 * ``fastqc_report.html`` is the visual HTML report showing plots of the
   QC metrics summarized in the text summary.
* ``summary.txt``: A tab-separated file describing whether the pass/warn/fail
  result for each module. If multiple files are provided, only one summary file
  is generated, with one of the columns being the file name associated to each
  module result.

the full list of arguments and options can be seen by running `falco` without any arguments, as well as `falco -?` or `falco --help`. This will print the following list:

```
Usage: ./bin/falco [OPTIONS] <seqfile1> <seqfile2> ...

Options:
  -h, --help                print this help file and exit
  -v, --version             print the program version and exit
  -o, --outdir              Create all output files in the specified
                            output directory. If notprovided, files
                            will be created in the same directory as
                            the input file.
  -C, --casava              Files come from raw casava output
                            (currently ignored)
  -n, --nano                Files come from fast5 nanopore sequences
  -F, --nofilter            If running with --casava do not sequences
                            (currently ignored)
  -e, --noextract           If running with --casava do not remove poor
                            quality sequences (currently ignored)
  -g, --nogroup             Disable grouping of bases for reads >50bp
  -f, --format              Force file format
  -t, --threads             Specifies number of threads to process
                            simultaneos files in parallel (currently
                            set for compatibility with fastqc. Not yet
                            supported!)
  -c, --contaminants        Non-default filer with a list of
                            contaminants
  -a, --adapters            Non-default file with a list of adapters
  -l, --limits              Non-default file with limits and warn/fail
                            criteria
  -T, --skip-text           Skip generating text file (Default = false)
  -H, --skip-html           Skip generating HTML file (Default = false)
  -S, --skip-short-summary  Skip short summary(Default = false)
  -q, --quiet               print more run info
  -d, --dir                 directory in which to create temp files
  -A, --advanced-mode       advanced mode: adds more information to the
                            FastQC output depending on non-fastqc user
                            flags
  -B, --bisulfite           reads are whole genome bisulfite
                            sequencing, and more Ts and fewer Cs are
                            therefore expected and will be accounted
                            for in base content (advanced mode)
  -R, --reverse-complement  The input is a reverse-complement. All
                            modules will be tested by swapping A/T and
                            C/G

Help options:
  -?, -help                 print this help message
      -about                print about message

PROGRAM: ./bin/falco
A high throughput sequence QC analysis tool
```

Citing falco
=============
If `falco` was helpful for your research, you can cite us as follows:

*de Sena Brandine G and Smith AD. Falco: high-speed FastQC emulation for
quality control of sequencing data. F1000Research 2021, 8:1874
(https://doi.org/10.12688/f1000research.21142.2)*

Copyright and License Information
=================================

Copyright (C) 2019
University of Southern California,

Current Authors: Guilherme de Sena Brandine, Andrew D. Smith

This is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free
Software Foundation, either version 3 of the License, or (at your
option) any later version.

This software is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
General Public License for more details.
