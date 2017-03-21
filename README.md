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
cat mysequences.fasta | grep ">" | wc –l
```

# Untaring multiple files in a folder

```bash
for file in *.gz; do tar -xvfz $file;done
```

# Annotating entries in a fasta file ( not quite one line)

Assuming we have an indexed blast database built as follows

```bash
makeblastdb -in uniprot_sprot.fasta -input_type 'fasta' -dbtype 'prot' -parse_seqids
```

Find homologs with blastp

```bash
blastp -query contigs.fasta -db uniprot_sprot.fasta -outfmt '6 qseqid sacc evalue stitle' -max_target_seqs 1 > contigs.blastp
```

Join blastp results to the fasta file. This just prints in tabular format

```bash
join <(bioawk -c fastx '{print $name,$seq}' contigs.fasta) <(awk -F '\t' '{print $1,$2,$3,$4}' contigs.blastp)
```

And finally reformat output to fasta with annotations

```bash
join <(bioawk -c fastx '{print $name,$seq}' contigs.fasta) <(awk -F '\t' '{print $1,$2,$3,$4}' contigs.blastp) | awk -F '\t' {printf(">%s %s\n%s\n",$1,$4,$2)}'
```
