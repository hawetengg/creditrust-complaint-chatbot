# CrediTrust Complaint-Answering RAG Chatbot

## Project Overview
This project develops an internal AI tool for CrediTrust Financial, leveraging Retrieval-Augmented Generation (RAG) to provide synthesized, evidence-backed answers to natural language questions about customer complaints from the CFPB database.

## Project Structure
```
creditrust-rag/
├── data/                  # Stores raw and processed datasets
├── notebooks/             # Jupyter notebooks for experimentation and detailed steps
├── src/                   # Python scripts for reusable functions and pipeline components
├── vector_store/          # Persisted FAISS index and associated metadata
├── reports/               # Evaluation results, summaries, and detailed reports
├── screenshots/           # UI screenshots (for final task)
├── .gitignore             # Specifies intentionally untracked files to ignore
├── README.md              # Project overview and documentation
└── requirements.txt       # List of project dependencies
```
## Setup and Installation
1.  **Clone the repository:**
    ```bash
    git clone https://github.com/hawetengg/creditrust-complaint-chatbot.git
    cd creditrust-complaint-chatbot
    ```
2.  **Create and activate a virtual environment:**
    ```bash
    python -m venv venv
    .\venv\Scripts\activate # On Windows PowerShell
    # For macOS/Linux: source venv/bin/activate
    ```
3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

## How to Run
Detailed instructions will be added here for each task as the project progresses.

### Running Task 1 (EDA & Preprocessing)
* **Via Notebook:** Open `notebooks/01_eda_preprocessing.ipynb` and run all cells.
* **Via Script:** Execute the preprocessing script.
    ```bash
    # This command assumes the preprocess.py script is designed to run the preprocessing
    # You might need to adjust arguments based on how your script is set up.
    python src/preprocess.py
    ```

### Running Task 2 (Embedding & Vector Store Indexing)
* **Via Notebook:** Open `notebooks/02_embedding_chunking.ipynb` and run all cells.
* **Via Script:** Execute the vector store creation script.
    ```bash
    # This command assumes the vector_db.py script is designed to run the indexing
    # Ensure filtered_and_cleaned_complaints.csv from Task 1 is available in data/
    python src/vector_db.py
    ```

## Project Progress and Detailed Tasks

### Task 1: Exploratory Data Analysis (EDA) & Data Preprocessing
**Objective:** To understand the structure, content, and quality of the CFPB complaint data and prepare it for the RAG pipeline.

**Implementation Details:**
* **Data Loading:** Loaded the full CFPB complaint dataset from `data/raw_complaints.csv`.
* **Initial EDA:**
    * Analyzed the distribution of complaints across different "Product" categories, identifying dominant complaint types.
    * Calculated and visualized the length (word count) of the "Consumer complaint narrative" column, noting the presence of very short and very long narratives, and managing missing values.
    * Identified the total number of complaints, including those with and without narrative text.
* **Dataset Filtering:**
    * Filtered the dataset to include only records pertaining to specific financial products relevant to the project, such as "Credit card", "Personal loan", "Buy Now, Pay Later", "Savings account", and "Money transfers". (Note: The filtering specifically included a broader set of related categories to ensure comprehensive coverage, like "Credit reporting or other personal consumer reports" due to their prevalence and relevance to core products).
    * Removed any records where the "Consumer complaint narrative" field was empty or contained only boilerplate text, ensuring that only meaningful narratives are processed.
* **Text Cleaning:** Implemented a robust text cleaning function for the "Consumer complaint narrative" column, which included:
    * Lowercasing all text.
    * Removing special characters, numbers, and common boilerplate phrases (e.g., "XXXX" placeholders).
    * Performing lemmatization to reduce words to their base forms (e.g., "running" to "run").
    * Removing common English stop words (e.g., "the", "is", "a") to focus on meaningful terms for embedding.

**Deliverables:**
* Jupyter Notebook: `notebooks/01_eda_preprocessing.ipynb`
* Python Script: `src/preprocess.py` (containing the core cleaning logic)
* Cleaned and Filtered Dataset: `data/filtered_and_cleaned_complaints.csv`

---

### Task 2: Text Chunking, Embedding, and Vector Store Indexing
**Objective:** To convert the cleaned text narratives into a format suitable for efficient semantic search within the RAG pipeline.

**Implementation Details:**
* **Text Chunking Strategy:**
    * Utilized LangChain's `RecursiveCharacterTextSplitter` to break down long complaint narratives into smaller, contextually relevant chunks.
    * Experimented with `chunk_size` (e.g., 500 characters) and `chunk_overlap` (e.g., 100 characters) to ensure chunks maintain coherence and context across boundaries, justifying the final choice in the interim report.
* **Embedding Model Choice:**
    * Selected `sentence-transformers/all-MiniLM-L6-v2` as the embedding model. This model was chosen for its excellent balance of performance (generating high-quality 384-dimensional embeddings) and efficiency (being relatively small and fast, suitable for CPU inference on large datasets).
* **Embedding Generation & Indexing:**
    * Generated vector embeddings for each text chunk using the chosen Sentence Transformer model.
    * Created a FAISS (Facebook AI Similarity Search) index (`IndexFlatIP`) to store these embeddings. FAISS was chosen for its speed and efficiency in similarity search over large vector datasets.
    * Crucially, alongside each vector, associated metadata (including the `Product` category and `Complaint ID`) was stored. This enables tracing a retrieved text chunk back to its original complaint and product, which is essential for providing evidence-backed answers.

**Deliverables:**
* Jupyter Notebook: `notebooks/02_embedding_chunking.ipynb`
* Python Script: `src/vector_db.py` (containing the core chunking, embedding, and indexing logic)
* Persisted Vector Store: `vector_store/faiss_index.bin` (the FAISS index) and `vector_store/metadata.json` (the associated metadata).

---

## Key Deliverables & Tasks (Summary)
- **Task 1: EDA & Preprocessing:** Cleaned dataset (`data/filtered_and_cleaned_complaints.csv`).
- **Task 2: Embedding & Vector Store:** Persisted vector index (`vector_store/faiss_index.bin`, `vector_store/metadata.json`).
- **Task 3: RAG Core & Evaluation:** RAG pipeline (`src/rag_pipeline.py`), evaluation table (`reports/evaluation_table.md`).
- **Task 4: Interactive UI:** Gradio/Streamlit app (`src/app.py`), UI screenshots (`screenshots/`).

---
Developed for CrediTrust Financial as a Data & AI Engineer.