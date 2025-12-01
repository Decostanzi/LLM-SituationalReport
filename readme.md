## Workflow Description

---

### 0. Data Selection

**Notebook:** `0-data-selection-countryPeriod.ipynb`

**Description:**

- **Loads the master Excel file** (`full_data.xlsx`).  
- Filters documents by **country + week**.  
- Applies **manual filtering** on document titles.  
- Adds metadata for all selected source files.  

**Input paths:**

- `full_data.xlsx`

**Output paths:**

- `/Results/Sources/SourcesCountryEvent/`  
- `/Results/Sources/SourcesCountryEvent-Metadata/`

---

### 1. Document Cleaning & Clustering

**Notebook:** `1-clean-clustering.ipynb`

**Description:**

- Loads the sources extracted in the previous step.  
- Splits each document into **paragraphs**.  
- Cleans and preprocesses text.  
- Performs **dimensionality reduction**.  
- Runs **HDBSCAN clustering**.  
- Uses **random parameter search** and evaluation metrics for optimization.  

**Input paths:**

- `/Results/Sources/SourcesCountryEvent-Metadata/`  
- `/Results/Sources/SourcesCountryEvent/`

**Output paths:**

- `/Results/paragraphs_metadata/`  
- `/Results/Cluster/Clusters/`  


---

### 2. Question Generation

**Notebook:** `2.0-QuestionsGeneration.ipynb`

**Description:**

- Generates **cluster-level questions** based on the methodology described in the paper.  
- Uses the metadata sources.  
- Deduplicates and filters out redundant questions.  

**Input paths:**

- `/Results/Sources/SourcesCountryEvent-Metadata/`

**Output paths:**

- `/Results/Questions/Test questions different prompts/1-Questions generated/Dev set`

&nbsp;

**Notebook:** `2.1-Questions_filtering_SDGs.ipynb`

**Description:**

- Loads the previously generated questions.  
- Filters the questions using evaluation metrics.  
- Classifies each question into relevant **SDGs**.  

**Input paths:**

- `/Results/Questions/Test questions different prompts/1-Questions generated/Dev set`

**Output paths:**

- `/Results/Questions/Test questions different prompts/3-Filtered questions/Dev set/`  
- `/Results/Questions/Test questions different prompts/4-Filtered questions with SDGs/Dev set`

---

### 3. Answers Extraction

**Notebook:** `3.0-Rag GeneratedQuestions.ipynb`

**Description:**

- Takes **filtered questions** and related paragraphs.  
- Runs a **RAG (Retrieval-Augmented Generation)** pipeline.  
- Produces answers enriched with **citations**.  

**Input paths:**

- `/Results/Questions/Test questions different prompts/3-Filtered questions/Dev set/`  
- `/Results/Questions/Test questions different prompts/1-Questions generated/Dev set/`

**Output paths:**

- `/Results/Answers/Answers-Subtopics/Dev set`

&nbsp;

**Notebook:** `3.3-RAG_PostProcessingCitations.ipynb`

**Description:**

- Cleans, updates, and refines citation formatting.  
- Ensures citation accuracy across all answers.  

**Input paths:**

- `/Results/Answers/Answers-Subtopics/Dev set`

**Output paths:**

- `/Results/Answers/Answers-Subtopics/Dev set/Updated_citations/`

---

### 4. Summary Generation

**Notebook:** `5-Summary for each paragraph.ipynb`

**Description:**

- Uses the RAG-processed paragraphs.  
- Produces a **cluster-level summary** describing each cluster in narrative form.  

**Input paths:**

- `/Results/Answers/Answers-Subtopics/Dev set/Updated_citations/`

**Output paths:**

- `/Results/Summaries/UniqueSummary-EachCluster/`

&nbsp;

**Notebook:** `5-SDG-Summary.ipynb`

**Description:**

- Sorts paragraphs and answers by **SDGs**.  
- Generates **SDG-level summaries**.  

**Input paths:**

- `/Results/Answers/Answers-Subtopics/Dev set/Updated_citations/`  
- `/Results/Questions/Test questions different prompts/4-Filtred questions with SDGs/`

**Output paths:**

- `/Results/Answers/Answers-SDGs`  
- `/Results/Summaries/UniqueSummary-EachSDG`

---

### 5. Executive Summary Generation

**Notebook:** `4-Executive summary generation.ipynb`

**Description:**

- Uses RAG-processed paragraphs.  
- Generates a concise **executive summary** for each event.  
- Produces a **headline** for each summary.  

**Input paths:**

- `/Results/Answers/Answers-subtopics/Dev set/Updated_citations`

**Output paths:**

- `/Results/Executive Summary/Dev set`

&nbsp;

**Notebook:** `4.1-Executive summary-PostProcess Citations.ipynb`

**Description:**

- Updates and refines citations in the executive summaries.  

**Input paths:**

- `/Results/Executive Summary/Dev set`

**Output paths:**

- `/Results/Executive Summary/Dev set/updated citations`

---

### 6. Report Visualization

**Notebook:** `6-Report Generation.ipynb`

**Description:**

- Generates the **final reports** for each event.  
- Integrates **all previously generated files**.  
- Produces both **Q&A-style** and **summary-style** reports.  
- Outputs both **Markdown** and **JSON** formats.  

**Input paths:**

- `./Results/Sources/SourcesCountryEvent/Dev set/`  
- `./Results/Cluster/Clusters+Headline`  
- `./Results/Answers/Answers-subtopics/Dev set/Updated_citations`  
- `./Results/paragraphs_metadata`  
- `./Results/Executive Summaries/Dev set/Updated_citations`  
- `./Results/Summaries/UniqueSummary-EachCluster/Dev set`

**Output paths:**

- `./Results/Reports/JSON_Report_QA/Dev set`  
- `./Results/Reports/Markdown_Report_QA/Dev set`  
- `./Results/Reports/JSON_Report_Summary/Dev set`  
- `./Results/Reports/Markdown_Report_Summary/Dev set`

&nbsp;

**Notebook:** `6-Report Generation-SDGs.ipynb`

**Description:**

- Generates the **SDG-arranged** versions of the final reports.  
- Produces both **QA** and **Summary** formats, in Markdown and JSON.  

**Input paths:**

- `./Results/Sources/SourcesCountryEvent/Dev set/`  
- `./Results/Cluster/Clusters+Headline`  
- `./Results/Answers/Answers-subtopics/Dev set/Updated_citations`  
- `./Results/paragraphs_metadata`  
- `./Results/Executive Summaries/Dev set/Updated_citations`  
- `./Results/Summaries/UniqueSummary-SDGs/Dev set`

**Output paths:**

- `./Results/Reports/JSON_Report_QA_SDGs/Dev set`  
- `./Results/Reports/Markdown_Report_QA_SDGs/Dev set`  
- `./Results/Reports/JSON_Report_Summary_SDGs/Dev set`  
- `./Results/Reports/Markdown_Report_Summary_SDGs/Dev set`

&nbsp;

**Notebook:** `6.1-Reports Postprocessing.ipynb`

**Description:**

- Postprocesses all reports to ensure **consistent, unique citation numbering**.  

**Input paths:**

- `./Results/Reports/JSON_Report_QA_SDGs/Dev set`  
- `./Results/Reports/JSON_Report_Summary_SDGs/Dev set`  
- `./Results/Reports/JSON_Report_QA/Dev set`  
- `./Results/Reports/JSON_Report_Summary/Dev set`

**Output paths:**

- Same as inputs, each with `_updated_citations` appended.



