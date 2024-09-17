# drug-screen-sample-results

## Goal:
This work focuses on a drug class of short DNA sequences targeting specific mRNAs within target cells. The purpose of this work was to ascertain the characteristics, whether sequential, structural or otherwise that improve the pharmacokinetics properties of this drug class. Various desired features were tested, including stability in biological media / animal models, cell penetration, aggregation propensities etc. 

## Method:
A vast combinatorical library of these potential drugs (10^10) was used, where each variation is represented a thousand times, was tested as a mixture and the output was measured using NextGen Sequencing. Time course measurements were taken from each experiment, after incubation with the drug library. Both treated an untreated control samples were used in each experiment, with three replicates for each. NGS analysis allowed for the simultaneous measurement of the entire drug library from each sample, in each experiment.

I was tasked with constructing the analysis pipeline, including the initial processing of the sequencing data, the mathematical, statistical and ML modeling anaylsis used to draw conclusions as to the prperties that control each desired trait and use these conclusions to construct a class of uber-drugs that contain these desired features. Below is a small sample of the analysis results.

 
## Sample Results:

The results below are from a drug stability assay in animal models. Samples were taken from various organs at different time points post administration, drug sequences were extracted and sequenced. 
Each DNA sequence was digitized using one-hot encoding and vectorized.  Below is a 2D projection after dimensionality reduction where each point on the 2D image represents a single drug variation. Results are shown for one of the replicates at 1hr post library administration.
For dimentionality reduction t-SNA was initially used, but UMAP was able to upscale with increasing sample data more effectively, and produced comparative results. Ultimately a 3D representation was used for downstream analysis, the 2D is given in this explanation due to its visual simplicity.
This “Jellyfish” pattern was consistent and appeared in the three replicates.

![image](https://github.com/user-attachments/assets/86e00df4-d958-435b-aa75-6a0a9ae1b9b2)

Below is the same 2D projection (slightly different scales of the X and Y axis), but with a different coloring scheme: On the right each data point is colored by its organ of origin. On the left points are colored by their nucleotide compositions, whether they contain more (>40%) G, T, A or C. The coloring schemes show that drug leads dampled from different organs are positioned in divergent patterns on the 2D projection, and that the nucleotide composition of each sequence could explain, at least in part, the source of these patterns. For example, drug sequences that were found in the blood (i.e. serum/plasma) are positioned on the right of the projection and are more rich in C. Conversely, drug sequences that were found in the kidneys are more enriched for G and T.

![image](https://github.com/user-attachments/assets/d034cb1a-ad22-4304-a024-85a16357db68)

Next I wanted to find more specific sequential patterns and structural motifs in the data and I used the OPTICS algorithm to cluster the data. OPTICS had many advantages for this analysis: It clusters based on density. It can adapt to different regions having different densities. It leaves outliers out of the cluster (as oppose to k-means for example). I don’t have to specify the numbers of cluster a priori, it adapts the number of clusters based on hyperparameters (as oppose to k-means). I can adapt the number of clusters and the amount of outliers to remove according to my specific needs. The left and right figure differ in hyperparametes used.
The flexibility offered by the OPTICS modeling was crucial. Since the next step of the analysis was the very resource-intensive structural modeling of each drug sequence I want to remove as many outliers as possible, choosing to focus more closely on the core of each cluster, in order to find the underlying structural motif. Below is a sample clustering pattern showing twelve different clusters, each with its corresponding color.

![image](https://github.com/user-attachments/assets/ea2aba77-4211-477b-8a86-ec9d9d3c05b8)

Next the secondary structure of each of the drug sequences in the clusters was calculated using the NUPACK package and a 3D approximation was modeled using FARFAR2 (Rosetta). The figure below shows a 3D visualization of modeling of two of these clusters, where gray ribbons show the superposition of the drug leads in the cluster and the rainbow ribbon show the mean structure calculated from all structures in the cluster. Structure standard deviation was used as a measurement for cluster homogeneity, where some cluster showed a more narrow distribution of structures, such as in the left example, suggesting a common structural motif. Where others showed a more chaotic superposition, reflected in a large structural standard deviation.

![image](https://github.com/user-attachments/assets/28180d2c-27f7-4033-bcb0-4f9d8afa3518)

Below is the representing structure, i.e. the least mean squared structure, of each of the major clusters.

![image](https://github.com/user-attachments/assets/fe64841c-6a14-4d9d-a0ca-fc43272ebb9c)

