# Lung Squamous Cell Carcinoma (LUSC) Gene Expression Analysis

This project performs an in-depth analysis of a TCGA Lung Squamous Cell Carcinoma (LUSC) gene expression dataset. The primary goals are to distinguish between tumor and normal samples, identify distinct cancer subtypes within the tumor samples, discover potential biomarkers for these subtypes, and build a machine learning model to classify them.

## Workflow

The analysis is structured into several key stages:

1. Data Preparation

- Loading Data: The gene expression data is loaded from LUSCexpfile.csv.

- Data Cleaning: Duplicate gene entries are handled by averaging their expression values.

- Feature Selection: To focus on the most informative genes, the dataset is filtered to keep the top 10,000 genes with the highest variance.

- Transformation & Normalization: The data undergoes a log2(TPM + 1) transformation to stabilize variance, followed by Z-score normalization to make expression levels comparable across samples.

2. Exploratory Data Analysis (EDA)

- Principal Component Analysis (PCA): PCA is used to get a high-level view of the data, showing a clear separation between tumor and normal samples. - Heatmap: A heatmap of the 50 most variable genes provides a visual representation of expression patterns, further highlighting the differences between tumor and normal tissues.

3. Differential Expression Analysis

- T-tests: A t-test is performed for each gene to identify statistically significant differences in expression between tumor and normal samples.

- FDR Correction: The Benjamini-Hochberg method is used to adjust p-values for multiple comparisons, controlling the False Discovery Rate (FDR).

- Volcano Plot: The results are visualized with a volcano plot, which displays the log2 fold change against the -log10 adjusted p-value, making it easy to spot significantly upregulated and downregulated genes.

4. Gene Set Enrichment Analysis (GSEA)

- The list of differentially expressed genes is analyzed using gseapy to identify enriched biological pathways from the KEGG and GO databases. This helps in understanding the functional implications of the expression changes.

5. Cancer Subtyping

- Elbow Method: This method is used on the tumor samples to determine the optimal number of clusters (k). The analysis suggested an optimal k of 4.

- K-Means Clustering: The tumor samples are grouped into four distinct subtypes using the K-Means algorithm.

- UMAP Visualization: The identified clusters are visualized in a 2D space using Uniform Manifold Approximation and Projection (UMAP), which reveals the structure and separation of the cancer subtypes.

6. Biomarker Discovery for Subtypes

- Differential Expression Between Subtypes: T-tests are run to find genes that are uniquely up- or down-regulated in each subtype compared to the others.

- Feature Importance with XGBoost and SHAP: An XGBoost model is trained to classify the subtypes, and SHAP (SHapley Additive exPlanations) is used to interpret the model. This reveals which genes are most predictive for each subtype.

- Biomarker Heatmap: A heatmap is generated to visualize the expression patterns of the top 50 biomarker genes across the identified subtypes.

7. Building and Evaluating a Subtype Classifier

- Model Training: A Random Forest classifier is trained to predict the cancer subtype based on gene expression profiles.

- Evaluation: The model's performance is assessed using accuracy, a classification report (including precision, recall, and F1-score), and a confusion matrix.

- Handling Class Imbalance: The initial model showed some bias due to an imbalanced number of samples in each subtype. The Random Forest model was retrained with class_weight='balanced' to address this, leading to an improved and more robust classifier.

##Key Results

- Differential Expression: 2,709 differentially expressed genes were identified between tumor and normal samples.

- Enriched Pathways: Top enriched pathways in tumor samples include Staphylococcus aureus infection, Cell adhesion molecules, and Cell cycle.

- Cancer Subtypes: The analysis successfully identified four distinct subtypes within the LUSC tumor samples.

Classifier Performance:

- Initial Accuracy: 92.05%

- Improved Accuracy (Balanced): 90.07% (While slightly lower in overall accuracy, the balanced model provides a better and more generalizable performance across all classes, especially for the underrepresented subtype 3).

