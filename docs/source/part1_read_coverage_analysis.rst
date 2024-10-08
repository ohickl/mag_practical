Part 1: Read Coverage Analysis with CoverM
==========================================

Overview
--------

In this part, you will calculate the coverage of your assembled contigs using CoverM. Coverage information is essential for metagenomic binning as it helps differentiate contigs based on their abundance.

Task 1: Prepare Input Files
---------------------------

**Step 1: Inspect the Assembly and Read Files**

List the assembly and mapping files:

.. code-block:: bash

   ls -lh assembly/
   ls -lh reads/

**Questions**

- **Q1:** What is the format of the assembly and the read file?

**Hints**

- The assembly file is usually in FASTA format.
- The read file(s) are typically in FASTQ format.

Task 2: Run CoverM to Calculate Coverage
----------------------------------------

**Step 1: Run CoverM**

Use CoverM to calculate per-contig coverage:

.. code-block:: bash

   # Map reads to the assembly
   coverm make --interleaved ${reads} \
               --reference ${assembly} \
               --threads 128 \
               --output-directory mapping

   # Set bam and coverage variables
   bam="${session_path}/mag_practical/mapping/mapping.bam"
   coverage="${session_path}/mag_practical/mapping/coverage.tsv"

   # Rename bam file for clarity
   mv mapping/assembly.fasta.reads.fq.bam ${bam}

   # Calculate coverage
   coverm contig --bam-files ${bam} \
                 --threads 128 \
                 --output-file ${coverage}

**Step 2: Inspect the Coverage Output**

View the first few lines of the coverage file:

.. code-block:: bash

   head -n 10 ${coverage}

**Questions**

- **Q3:** What columns are present in the coverage output?
- **Q4:** What does the 'mean' coverage represent?

**Step 3: Analyze Coverage Distribution**

Create a histogram of coverage values:

.. code-block:: bash

   # Extract coverage values
   cut -f2 ${coverage} | tail -n +2 > mapping/coverage_values.txt
   # Plot the histogram
   Rscript -e "hist <- read.table('mapping/coverage_values.txt', header=FALSE); hist(hist\$V1, breaks=50, main='Coverage Distribution', xlab='Coverage')"

   # Rename the plot file
   mv Rplots.pdf mapping/coverage_distribution.pdf

**Questions**

- **Q5:** What does the coverage distribution tell you about the abundance of different contigs?
- **Q6:** Are there contigs with exceptionally high or low coverage?

Task 3: Interpret the Coverage Data
-----------------------------------

**Step 1: Identify Outliers**

- **Q7:** How can you identify contigs that may be outliers based on coverage?
- **Q8:** What might be the reasons for unusually high or low coverage in some contigs?

**Step 2: Consider Biological Implications**

- **Q9:** How does coverage information assist in differentiating contigs during binning?
- **Q10:** Why is it important to have accurate coverage data before binning?

**Notes**

- Coverage is a proxy for the abundance of the organism from which the contig originated.
- Contigs from the same genome should have similar coverage patterns.

