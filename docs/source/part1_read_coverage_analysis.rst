Part 1: Read Coverage Analysis with CoverM
==========================================

Overview
--------

In this part, you will calculate the coverage of your assembled contigs using CoverM. Coverage information is essential for metagenomic binning as it helps differentiate contigs based on their abundance.

Task 1: Calculate the Mean Read Coverage of Contigs
----------------------------------------

**Step 1: Run CoverM**

Use CoverM to calculate per-contig coverage:

.. code-block:: bash

   # Map reads to the assembly
   coverm make --interleaved ${reads} \
               --reference ${assembly} \
               --threads ${threads} \
               --output-directory mapping

   # Set bam and coverage variables
   bam="${session_path}/mag_practical/mapping/mapping.bam"
   coverage="${session_path}/mag_practical/mapping/coverage.tsv"

   # Rename bam file for clarity
   mv mapping/assembly.fasta.reads.fq.bam ${bam}

   # Calculate coverage
   coverm contig --bam-files ${bam} \
                 --threads ${threads} \
                 --output-file ${coverage}

**Step 2: Inspect the Coverage Output**

View the first few lines of the coverage file:

.. code-block:: bash

   head -n 10 ${coverage}

**Questions**

- **Q1:** What columns are present in the coverage output?
- **Q2:** What does the 'mean' coverage represent?

**Step 3: Analyze Coverage Distribution**

Create a histogram of coverage values:

.. code-block:: bash

   # Extract coverage values
   cut -f2 ${coverage} | tail -n +2 > mapping/coverage_values.txt
   # Plot the histogram
   Rscript -e "hist <- read.table('mapping/coverage_values.txt', header=FALSE); hist(hist\$V1, breaks=50, main='Coverage Distribution', xlab='Coverage')"

   # Rename the plot file
   mv Rplots.pdf mapping/coverage_distribution.pdf

.. hint::

   If you cant download the plot file, you can get one from from Slack.

**Questions**

- **Q3:** Are there contigs with exceptionally high or low coverage?
- **Q4:** How does coverage information assist in differentiating contigs during binning?
- **Q5:** Why is it important to have accurate coverage data before binning?

.. hint::

   .. toggle:: **Notes**

      - Coverage is a proxy for the abundance of the organism from which the contig originated.
      - Contigs from the same genome should have similar coverage patterns.

