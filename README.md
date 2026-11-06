Police-Reported Cybercrime in Canada: K-Means Clustering
PROG8431 — Data Analysis, Mathematics, Modeling and Algorithms
Problem Analysis Workshop B | Group 2
Prepared by: Senay Teweldebrhan, Juan Camilo Chirivi, and Zeynep Ozdemir
Project overview
This project uses K-Means clustering to examine patterns in police-reported cybercrime across Canadian Census Metropolitan Areas (CMAs). Using Statistics Canada data, we compare each CMA's 2025 cybercrime rate per 100,000 population with its percentage change in that rate between 2023 and 2025.

The goal is to identify groups of metropolitan areas with similar combinations of current reported rates and recent changes. Clustering helps reveal patterns that are not immediately obvious from ranking CMAs by a single statistic.
Research question: Which Canadian CMAs have experienced the largest recent increases in police-reported cybercrime rates, and what groups emerge when we compare their 2025 rates with percentage changes from 2023 to 2025?

This is an exploratory, unsupervised learning analysis, not a prediction or a time-series forecasting model.
Team members
Team member	Student ID
Senay Teweldebrhan	9120588
Juan Camilo Chirivi	9115141
Zeynep Ozdemir	9045142


Dataset and source
- Publisher: Statistics Canada
- Table: 35-10-0002-01 (downloaded file identifier: 35100002)
- Source: Statistics Canada — Table 35-10-0002-01
- Frequency: Annual
- Geographic unit used: Full Census Metropolitan Area (CMA)
- Study years: 2023 and 2025
- Records in the final clustering analysis: 39 CMAs with usable observations for both years
- Relevant source statistics: Rate per 100,000 population and, for additional cluster context, Number of cybercrime incidents
The analysis uses rates for clustering rather than raw incident totals, because CMA populations differ. We exclude records representing separate provincial portions of cross-border CMAs to avoid treating these parts as separate complete CMAs.

The raw source file is named 35100002.csv in the instructions below. If your downloaded file has a suffix such as 35100002(2).csv, rename it before running the notebook.
Methods
The notebook follows the main stages of the course's Week 7 clustering demonstration.
1. Data preparation
1. Load the Statistics Canada CSV into pandas.
2. Keep the 2023 and 2025 records where Statistics is Rate per 100,000 population.
3. Filter to full CMAs and exclude provincial-part entries.
4. Check for duplicate CMA/year combinations.
5. Pivot the data to one row per CMA, with separate columns for 2023 and 2025 rates.
6. Remove rows without a usable rate for both years; check the 2023 baseline before calculating percentage growth.
2. Features used for clustering
Exactly two numeric variables define the clusters:
Feature	Meaning	Why it matters
Rate_2025	Police-reported cybercrime rate per 100,000 population in 2025	Describes the current reported rate
Growth_2023_2025_pct	Percentage change in the rate from 2023 to 2025	Describes the relative direction and magnitude of recent change


The growth feature is calculated as:
\[
\text{Growth (\%)} = \frac{\text{Rate}_{2025} - \text{Rate}_{2023}}{\text{Rate}_{2023}} \times 100
\]
The baseline must be greater than zero for this formula to be defined. Because the percentage is based on the 2023 value, small baselines can produce large percentage changes.

Note: The 2025 incident count is used only to add context to the final summary. It is not a third clustering feature.
3. Exploratory data analysis
Before clustering, the notebook examines descriptive statistics, feature correlation, distributions, boxplots, and CMAs with the largest positive and negative percentage changes. Potential outliers are investigated rather than removed automatically.
4. Standardization
The two features have different units and numeric ranges. To prevent either feature from dominating Euclidean distances just because of its scale, we apply the Z-score transformation demonstrated in class:
\[
z = \frac{x - \bar{x}}{s}
\]
Here, $\bar{x}$ is the sample mean and $s$ is the sample standard deviation. The notebook verifies that the standardized features have means approximately equal to 0 and sample standard deviations approximately equal to 1.
5. K-Means and model selection
The notebook fits K-Means models for K = 1 through 7, using:
KMeans(n_clusters=k, max_iter=10, n_init=10, random_state=123)
For each K, it reports cluster centers and sizes, alongside total sum of squares (TSS), within-cluster sum of squares (WSS / inertia) and between-cluster sum of squares (BSS = TSS − WSS). An elbow chart is used to assess changes in WSS. Scatter plots compare K = 3 against K = 4, with centroids displayed in the features' original units.

The final recommendation is K = 3, based on the decline in WSS and the interpretability of the resulting groups. K = 4 is also considered a reasonable, more detailed alternative; the elbow is not perfectly sharp.
Results and findings
Selected model: three clusters
Cluster profile	CMAs	Share of CMAs	Mean 2025 rate per 100,000	Mean change, 2023–2025
Lower rates / decline	25	64.10%	235.71	−15.91%
High rates / near-stable	6	15.38%	627.32	−0.16%
Strong growth	8	20.51%	272.99	+38.68%
Total	39	100%	—	—


Rates and percentage changes are unweighted averages of the CMAs assigned to each group. The labels describe group-level tendencies; individual CMAs may differ. K-Means numeric cluster IDs are arbitrary, so interpret groups using their descriptive names.
Main insights
- High current rates and rapid growth are different patterns. The group with the highest average 2025 rate is not the group with the strongest average growth.
- Most CMAs belong to the lower-rate/decline group. This group contains 25 of the 39 CMAs studied.
- The strong-growth group warrants closer investigation. Its average increase from 2023 to 2025 is 38.68%, even though its average 2025 rate is well below that of the high-rate group.
- Individual CMA examples show why both features matter. In the notebook, Guelph has the largest percentage increase (60.50%), while Abbotsford–Mission has the highest 2025 rate (893.6 per 100,000).
- Incident totals add useful context. They are included in the cluster summary but do not determine cluster membership.
These observations describe patterns in the analyzed dataset; they do not establish causes or rank the safety of metropolitan areas.
Repository structure
The recommended arrangement is:
K_Means_Clustering/
├── PROG8431_Workshop_B_KMeans_Clustering.ipynb
├── README.md
├── requirements.txt
├── .gitignore
└── data/
    ├── 35100002.csv
    └── 35100002_MetaData.csv
Important: The current .gitignore contains data/, meaning Git normally excludes this folder. If your team is permitted to redistribute the files and wants a fully self-contained repository, revise .gitignore accordingly and commit them. Otherwise, keep the files out of Git and use the download instructions below. Do not assume that downloading the repository alone will provide the raw data.
Getting started
Prerequisites
Install Python 3 and a Jupyter environment. The project uses pandas, NumPy, Matplotlib, scikit-learn, and IPython/Jupyter. The provided requirements.txt also lists seaborn and nbconvert.
1. Clone the repository
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd K_Means_Clustering
Replace <YOUR_GITHUB_REPOSITORY_URL> with your team's actual repository URL.
2. Create and activate a virtual environment (recommended)
Windows (PowerShell):
python -m venv .venv
.\.venv\Scripts\Activate.ps1
macOS / Linux:
python3 -m venv .venv
source .venv/bin/activate
3. Install the required packages
python -m pip install -r requirements.txt
4. Obtain and place the data
Download Statistics Canada Table 35-10-0002-01 using the dataset link in the Dataset and source section. Save the data CSV as:
data/35100002.csv
You may also place the accompanying metadata file at data/35100002_MetaData.csv for documentation. The notebook does not directly load that metadata file.

The notebook currently checks these CSV paths, in order:
Path("data/35100002.csv")
Path("../data/35100002.csv")
Path("35100002(1).csv")
The recommended path is data/35100002.csv relative to the repository root. If working from another directory, launch Jupyter from the repository root or adjust the file path.
5. Run the notebook
Start Jupyter using your preferred interface, for example:
jupyter notebook
Open PROG8431_Workshop_B_KMeans_Clustering.ipynb and select Kernel → Restart & Run All (wording may vary by Jupyter interface). Run all cells in order; later cells depend on variables created earlier.

The notebook displays preprocessing checks, plots, K-Means evaluation results, a comparison of K = 3 and K = 4, and the final named cluster profiles.
Reproducibility notes
- The notebook uses random_state=123 and n_init=10 to make K-Means initialization more consistent across runs.
- The final clustering uses 39 CMAs, two standardized features, and K = 3.
- Exact output and plots may depend on the downloaded data version and installed package versions; requirements.txt currently does not pin versions.
- Confirm that the CSV contains both 2023 and 2025 data and the source statistic labels expected by the notebook.
- If a file-not-found error appears, check the filename, folder, and Jupyter working directory first.
Limitations and responsible interpretation
1. Reported incidents are not all incidents. Reporting and recording practices can affect comparisons.
2. Geographic scope is limited. These 39 CMAs do not represent every community in Canada.
3. Growth can be sensitive to the baseline. A low 2023 rate can produce a large percentage change.
4. K-Means depends on analytical choices. Feature selection, scaling, outliers, and the number of clusters can influence the results.
5. Cluster profiles summarize, rather than explain, outcomes. Groups do not imply causal relationships, forecasts, or conclusions about the safety of a city.
6. Cluster averages are not population-weighted. Mean CMA rates should not be interpreted as an aggregate Canadian cybercrime rate.
Workshop presentation and submission
This project was prepared for Problem Analysis Workshop B in PROG8431. The workshop requires a seven-minute presentation using the submitted Jupyter Notebook, rather than separate PowerPoint slides. Every team member should be able to explain the data preparation, both variables, Z-score scaling, the elbow chart, the K = 3 decision, and the cluster summaries.

Submission reminder: Submit the .ipynb file (not a ZIP archive) to the designated assignment folder and include the GitHub repository link as requested by the instructor.
Acknowledgments
- Statistics Canada — source of the police-reported cybercrime dataset.
- PROG8431 course materials — Week 7 clustering demonstration and the Workshop B assignment instructions.
- Project team — shared data preparation, analysis, documentation, and presentation.
Academic project | PROG8431 — Problem Analysis Workshop B | Group 2