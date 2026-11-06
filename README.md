Police-Reported Cybercrime in Canada — K-Means Clustering
PROG8431 — Data Analysis, Mathematics, Modeling and Algorithms
Problem Analysis Workshop B | Group 2

GitHub Repository: https://github.com/tewelde05/K_Means_Clustering.git
Team Members
Name	Student ID
Senay Teweldebrhan	9120588
Juan Camilo Chirivi	9115141
Zeynep Ozdemir	9045142


Project Overview
This workshop applies K-Means clustering to police-reported cybercrime data from 39 Canadian Census Metropolitan Areas (CMAs). Building on our term-project analysis, we investigate which areas have similar current cybercrime rates and changes over time.

Research question: How can Canadian CMAs be grouped by their 2025 cybercrime rates and their percentage change from 2023 to 2025?

Data source: Statistics Canada, Table 35-10-0002-01. We use reported cybercrime rates per 100,000 people for 2023 and 2025, with 2025 incident counts for additional context.
Analysis and Methodology
The Jupyter Notebook documents each step, including the reasoning and visualizations required for Workshop B:
1. Clean and prepare: Select the relevant years and cybercrime measures, retain complete CMAs, reshape the data, and check for missing values and duplicates.
2. Choose two variables:
   - 2025 cybercrime rate (per 100,000 people) — shows the current level.
   - 2023–2025 percentage change in the rate — shows whether the rate increased or decreased.
3. Explore and standardize: Review descriptive statistics, histograms, and boxplots; then apply Z-score standardization so the two variables have comparable influence on K-Means distances.
4. Find the number of clusters: Run K-Means for K = 1 to 7, examine the elbow chart (within-cluster sum of squares), and compare scatter plots for K = 3 and K = 4.
5. Summarize and interpret: Choose K = 3 for a useful balance between cluster separation and interpretability; name the clusters and describe their patterns using rates, percentage changes, and incident counts.
Results
Cluster	Description	CMAs	Mean 2025 rate*	Mean change (2023–2025)
0	Lower rates / decline	25	235.71	−15.91%
1	High rates / near-stable	6	627.32	−0.16%
2	Strong growth	8	272.99	+38.68%


* Rate per 100,000 people. Figures are unweighted averages across CMAs in each cluster.

Main insight: CMAs with the highest current cybercrime rates are not necessarily those with the fastest growth. Clustering reveals different patterns, but does not establish causes or predict future rates.
How to Run the Notebook
1. Install Python 3 and clone the repository:
   git clone https://github.com/tewelde05/K_Means_Clustering.git
   cd K_Means_Clustering
2. Create a virtual environment in the project folder:
   # Windows
   python -m venv .venv
   # macOS / Linux
   python3 -m venv .venv
3. Activate the environment:
   # Windows (PowerShell)
   .\.venv\Scripts\Activate.ps1
   # Windows (Command Prompt)
   .venv\Scripts\activate
   # macOS / Linux
   source .venv/bin/activate
4. Install dependencies:
   python -m pip install -r requirements.txt
5. Download the dataset from Statistics Canada Table 35-10-0002-01 and save it as data/35100002.csv (create data/ if needed).
6. Run the notebook:
   jupyter notebook
   Open PROG8431_Workshop_B_KMeans_Clustering.ipynb and choose Restart & Run All.
When finished, run deactivate. Keep .venv/ and data/ in .gitignore so they are not uploaded to GitHub.
Workshop Submission and Presentation
The notebook contains the data preparation, feature selection, standardization, elbow chart, scatter plots, cluster summaries, and interpretations required by the workshop. We use the same submitted notebook for the seven-minute team presentation.

Submit the .ipynb file (not zipped) to the assignment folder and provide the GitHub repository link separately, as required by the instructor.
AI Assistance Acknowledgment
We used OpenAI ChatGPT to help review the Jupyter Notebook and improve the wording and organization of this README. Our team is responsible for checking the accuracy of the code, analysis, results, and final submission.