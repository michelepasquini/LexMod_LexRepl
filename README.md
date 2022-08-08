# LexMod_LexRepl
Python code to compute fundamental quantities in the evolution of a language family, as illustrated in the paper 
"Gradual modifications and abrupt replacements: two stochastic lexical ingredients of languages evolution" 
by Michele Pasquini, Maurizio Serva and Davide Vergni

Requires LingPy 2.6.9 package (https://lingpy.org/)

Directories
---------------------------------------------------------------------------
cldf    (intent: input)   datasets in cldf format
meg     (intent: output)  phylogenetic trees in MEGA format (https://www.megasoftware.net)
tabular (intent: output)  various tabular data in text format
tsv     (intent: output)  datasets in tsv format after Lexstat-Infomap algorithm

Scripts (in order of execution)
---------------------------------------------------------------------------
frequency.py  [ FILE_METADATA  [ NUMBER_OF_ITERATIONS ] ]
              Reads the dataset (via FILE_METADATA) and calculates:
            - the percentage frequency of NLD distribution for pairs of words with different concepts
              with NUMBER_OF_ITERATIONS random pairs (output: tabular/frequency_NLD_different_concepts.txt)
            - the percentage frequency of NLD distribution for pairs of words with the same concepts
              (output: tabular/frequency_NLD_same_concepts.txt)

threshold.py  [ FILE_METADATA  [ MIN_FREQUENCY ] ]
              Reads the dataset (via FILE_METADATA) and:
            - sums tabular/frequency_NLD_different_concepts.txt on appropriate NLD intervals of, at least,
              MIN_FREQUENCY absolute frequence (output: tabular/intervals_NLD_different_concepts.txt)
            - searches the optimal threshold for PSV cognate detection algorithm (see the paper) computing
              the LeCam distance between the resulting NLD distribution of non-cognate pairs and the target
              distribution tabular/intervals_NLD_different_concepts.txt (output: tabular/Le_Cam_distance.txt)

distances.py  [ FILE_METADATA  [ THRESHOLD ] ]
              Reads the dataset (via FILE_METADATA), run the PSV algorithm with threshold = THRESHOLD and:
            - calculates the percentage frequency of NLD distribution for pairs of cognate words
              (output: tabular/frequency_NLD_cognates.txt)
            - compares modifications distance matrix and replacements distance matrix
              (output: tabular/DR_vs_DM.txt)
            - exports some genealogical distances (T_NLD, T_M, T_R, T_RM, see the paper) in meg format
              to build up the phylogenetic trees with MEGA softwate (https://www.megasoftware.net)
              (output: meg/T_NLD.meg ; meg/T_M.meg ; meg/T_R.meg ; meg/T_RM.meg)

lexstat.py    [ FILE_METADATA ]
              Reads the dataset (via FILE_METADATA) and runs the Lexstat-Infomap algorithm (List et al. 2017)
              (output:  tsv/lexstat.tsv)

comparison.py [ THRESHOLD ]
              Reads the tsv/lexstat.tsv dataset and runs the PSV algorithm with threshold = THRESHOLD; then:
            - exports the genealogical distances T_R_infomap (output: meg/T_R_infomap.meg); then
            - calculates the B-cubed F-scores (Amigó et al. 2009) to compare the two cognate detection
      	      algorithms (Lexstat-Infomap and PSV)

library.py    A collection of useful functions
