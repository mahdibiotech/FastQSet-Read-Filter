# FastQSet-Read-Filter
This project provides a comprehensive set of tools to manipulate and filter biological sequence data from FASTQ files
# Projet 3: FastQSet Read Filter

## Overview

This project provides a comprehensive set of tools to manipulate and filter biological sequence data from FASTQ files. The primary classes and filters implemented allow for flexible data processing based on sequence quality, specific patterns (kmers), and other customizable criteria. The project includes:

- **Read class**: Represents a single FASTQ read with sequence, quality, and metadata.
- **FastQSet class**: Manages a collection of reads loaded from a FASTQ file.
- **Filter classes**: Various filter implementations for processing reads according to quality, kmers, and other patterns.

## Features

1. **Read Class**:
   - Represents individual sequence reads.
   - Accessors for read ID, sequence, quality scores, and description.
   - Quality scores are converted using the Phred33 scale.
   - Supports finding max, min quality, and nucleotide occurrence counts.

2. **FastQSet Class**:
   - Reads and stores data from a FASTQ file.
   - Provides methods to:
     - Get the number of reads.
     - Add new reads.
     - Access specific reads by index.
   - Filter reads using custom filter objects.
   - Export filtered reads to a new FASTQ file.

3. **Filter Classes**:
   - **Filter**: Base class with default `accept` method.
   - **QualityFilter**: Filters reads below a minimum quality.
   - **KmerFilter**: Excludes reads containing a specified k-mer.
   - **BiasKmerFilter**: Filters based on k-mer bias between sequences and their reverse complements.
   - **NFilter**: Filters reads based on a maximum number of 'N' nucleotides.
   - **PolyXFilter**: Filters reads containing excessively long stretches of repeated nucleotides.
   - **MultipleFilter**: Applies multiple filters in combination.

## Dependencies
- Python 3.6+
- No additional libraries are required.

## Usage

### Example FASTQ file (ownexample.fastq)

### Commands and Example Usage

1. **Loading a FASTQ File**:
   ```python
   fqset = FastQSet("ownexample.fastq")
   print(fqset.size())  # Outputs the number of reads
   ```

2. **Using Filters**:
   - **QualityFilter**: Minimum quality of 20.
     ```python
     quality_filter = QualityFilter(min_quality=20)
     filtered = fqset.filter_reads(quality_filter)
     ```
   - **KmerFilter**: Excludes reads containing the k-mer "ATCG".
     ```python
     kmer_filter = KmerFilter(kmer="ATCG")
     filtered = fqset.filter_reads(kmer_filter)
     ```
   - **NFilter**: Keeps reads with up to 5 'N's.
     ```python
     n_filter = NFilter(max_N=5)
     filtered = fqset.filter_reads(n_filter)
     ```

3. **Exporting Filtered Reads**:
   ```python
   fqset.filtered_reads("filtered_reads.fastq")
   ```

### Script: `filter.py`

This script demonstrates the usage of the various filters:

- Loads a FASTQ file and prints the total number of reads.
- Applies individual filters:
  - **QualityFilter**: Minimum quality of 20.
  - **KmerFilter**: Filters reads containing "ATCG".
  - **BiasKmerFilter**: Uses k-mer size range (3-5) and max difference 0.2.
  - **NFilter**: Filters reads with up to 5 'N's.
  - **PolyXFilter**: Filters poly-A stretches of 10 or more.
  - **MultipleFilter**: Combines Quality and Kmer filters.

#### Command Line Example
```bash
python filter.py
```

#### Sample Output
```
Ligne pour l'identifiant @MG01HX03:414:HHK35CCX2:4:1101:30990:60237 1:N:0:AACATCGC+CCTTGTTA
Ligne de la séquence TCCTGGAGCCATCCGCAGTTCGAGAAATCTCCGTTACAGATTTACTTGGAATCAATCTTGGGAGACGATGAATGGAGCTCAACTTATGAAGCAATCGATCCGGTTGTGCCGCCGATGCATTGGAACGAGGCTGGGAAAATTTTCCAACCGC
Ligne pour la description +
Ligne pour le score qualité:  AAFFFJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJFJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJ
Ligne pour l'identifiant @MG01HX03:414:HHK35CCX2:4:1101:8471:60255 1:N:0:AACATCGC+CCTTGTTA
Ligne de la séquence TCCTGGAGCCATCCGCAGTTCGAGAAACGTACATATGCCGAACAGGATTTTCGTGTGGGAGGTACCCGCTGGCATCGTCTGTTGCGCATGCCGGTCCGCGGACTGGATGGCGATAGTGCGCCGCTGCCGCCGCATACTACGGAACGCATTG
Ligne pour la description +
Ligne pour le score qualité:  AAFFFJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJ
Lecture terminée: Tous les reads ont été chargés
Total number of reads in the FastQSet: 25000
Reads after Quality Filter (min_quality=20): 17148
Reads after Kmer Filter (excluding k-mer 'ATCG'): 15347
Reads after Bias Kmer Filter (min_k=3, max_k=5, max_diff=0.2): 25000
Reads after N Filter (max_N=5): 25000
Reads after Poly-X Filter (poly-A length >= 10): 25000
Reads after applying Multiple Filters (Quality + Kmer): 10893
Filtered reads have been exported to 'filtered_quality.fastq'.
```

#### Comments
The results of the analysis indicate a comprehensive filtering process applied to a dataset of 25,000 sequencing reads. After quality filtering with a minimum quality score of 20, 17,148 reads remained, suggesting that approximately 31.4% of the reads did not meet this stringent quality threshold. The k-mer filter, designed to exclude reads containing the sequence "ATCG," further reduced the dataset to 15,347 reads, demonstrating the prevalence of this k-mer within the sample. All reads passed both the bias k-mer filter and the filters for ambiguous nucleotides (N) and homopolymeric runs, reflecting balanced strand representation and minimal sequencing artifacts in these aspects. The combined quality and k-mer filters retained 10,893 reads, representing a refined subset of sequences with high confidence for downstream analysis. The export to 'filtered_quality.fastq' was successfully completed, encapsulating this filtered set.



## Conclusion
In conclusion, the code effectively implements multiple filtering criteria to refine sequencing data, balancing stringency and inclusivity. The high initial rejection rate from the quality filter highlights the importance of choosing appropriate quality thresholds based on sequencing platform characteristics. While the k-mer filtering strategy is useful for detecting repetitive or contaminant sequences, careful parameter tuning is recommended to preserve data integrity. The overall workflow demonstrates robust data parsing, filtering, and export capabilities, offering a solid foundation for sequence quality control.
