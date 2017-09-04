# One line scripts for bioinformatics

# Extracting fasta records

Use [samtools faidx](http://www.htslib.org/doc/samtools.html)

First index the fasta file

```bash
samtools faidx mysequences.fasta
```

Then retrieve sequences by ID 

```bash
samtools faidx mysequences.fasta id1 id2 id3
```

# Reformatting fasta records

A great tool for this is **[bioawk](https://github.com/lh3/bioawk)** .  

For example to add a fasta compatible prefix like this

	>comp12345_c0_seq1
	to
	>lcl|comp12345_c0_seq1

can be done with the following bioawk command

```bash
bioawk -c fastx '{printf ">lcl|%s\n%s\n", $name, $seq}' original.fasta > reformatted.fasta
```

# Counting the number of sequences in a fasta file

```bash
grep -c ">" mysequences.fasta
```

# Untaring multiple files in a folder

```bash
for file in *.gz; do tar -xvfz $file;done
```
