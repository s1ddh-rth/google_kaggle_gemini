# 💼 Resume Reviewer – Job Match Assistant

## 📌 Project Overview

This project is a Generative AI-powered assistant designed to help job seekers optimize their application materials. It analyzes a user's resume in conjunction with a specific job description to provide actionable feedback and generate a personalized cover letter. The goal is to help users understand their resume's alignment with a job, identify areas for improvement, and streamline the application process.

## ✨ Key Features & GenAI Capabilities

This project demonstrates several key Generative AI capabilities:

1.  **Long-Context Handling:** Processes and understands the combined information from both the resume and the job description.
2.  **Few-Shot Prompting:** Utilizes examples to guide the AI model in generating desired outputs, such as specific resume edit suggestions and cover letter phrasing.
3.  **Structured Output (JSON):** Delivers results in a clean, parseable JSON format, making it easy to integrate with other tools or visualize the analysis.
4.  **Document Understanding:** Effectively analyzes and extracts relevant information from resumes (PDF/TXT) and job descriptions (PDF/HTML/TXT).
5.  **Retrieval-Augmented Generation (RAG):** Employs a RAG pipeline to find the most relevant sections from the resume and job description to inform the AI's analysis and suggestions. This involves:
    * Embedding text chunks from both documents.
    * Using FAISS for efficient similarity search to retrieve relevant context for a given query (e.g., "how well does my resume match?").
6.  **Context Caching:** Implements caching for extracted text and embeddings to avoid redundant processing and API calls, saving time and resources.

## 🔧 Technologies Used

* **Google Gemini API (`google-generativeai`):** For the core generative AI capabilities, including text embedding and content generation.
* **FAISS (`faiss-cpu`):** For efficient similarity search in the RAG pipeline.
* **PDFMiner (`pdfminer.six`) & PyPDF2:** For extracting text from PDF documents.
* **Beautiful Soup (`bs4`):** For parsing HTML content from job description URLs.
* **Joblib:** For caching processed data (text extractions, embeddings).
* **Python:** The primary programming language.
* **Jupyter Notebook:** For development and demonstration.

## ⚙️ How It Works

The assistant follows a multi-step process:

1.  **Data Input & Preprocessing:**
    * Accepts a resume and a job description (either as URLs or local paths).
    * Fetches and extracts text content from various formats (PDF, HTML, TXT).
    * Attempts to parse the resume and job description into logical sections (e.g., Summary, Experience, Skills for resumes; DESCRIPTION, QUALIFICATIONS for job descriptions).

2.  **Embedding & Indexing (RAG Setup):**
    * Chunks the extracted text from the resume and job description.
    * Generates embeddings for these text chunks using the Google Gemini API (`text-embedding-004` model).
    * Creates FAISS indexes for both resume and job description embeddings to enable quick retrieval of relevant information.

3.  **Analysis & Suggestion Generation:**
    * When a query is made (e.g., "How well does my resume match the job description?"), the system:
        * Embeds the query.
        * Uses the FAISS indexes to retrieve the most relevant text chunks from both the resume and the job description (RAG).
        * Constructs a detailed prompt for the Gemini model, including:
            * The retrieved relevant contexts (RAG results).
            * The full parsed resume and job description.
            * Few-shot examples for desired output format (e.g., edit suggestions, cover letter structure).
            * A pre-computed initial match score based on embedding similarity.
    * Sends the prompt to the Gemini model (`gemini-2.0-flash` or similar) to generate:
        * A match score.
        * Specific suggestions for resume improvements, categorized by section.
        * A personalized cover letter.

4.  **Structured Output:**
    * The final output is presented in a structured JSON format, including the match score, a list of suggestions (each with a section and an action), and the generated cover letter.

## 🚀 Getting Started

### Prerequisites

Ensure you have Python installed. You will also need a Google API Key with access to the Gemini API.

### Installation

1.  Clone the repository:
    ```bash
    git clone git@github.com:s1ddh-rth/google_kaggle_gemini.git
    ```
2.  Install the required Python libraries from the notebook, typically including:
    ```bash
    pip install google-generativeai faiss-cpu pdfminer.six PyPDF2 beautifulsoup4 joblib jupyterlab
    ```
    *(Refer to the initial cells of the Jupyter Notebook for the exact pip install commands used.)*

3.  Set up your Google API Key:
    * The notebook uses `kaggle_secrets` to load the API key. If running locally, you'll need to modify the API key loading mechanism (e.g., using environment variables or a `.env` file).
    ```python
    # Example of how it's done in the notebook (modify for local use if needed)
    # from kaggle_secrets import UserSecretsClient
    # GOOGLE_API_KEY = UserSecretsClient().get_secret("GOOGLE_API_KEY")
    # import google.generativeai as genai
    # genai.configure(api_key=GOOGLE_API_KEY)

    # For local use, you might do:
    import os
    import google.generativeai as genai
    GOOGLE_API_KEY = os.environ.get("GOOGLE_API_KEY")
    genai.configure(api_key=GOOGLE_API_KEY)
    ```

### Running the Notebook

1.  Launch Jupyter Lab:
    ```bash
    jupyter lab
    ```
2.  Open the `gen-ai-intensive-course-capstone-2025q1.ipynb` notebook.
3.  Modify the `resume_src` and `jd_src` variables in the "Data Upload + Preprocessing" section to point to your resume and the target job description:
    ```python
    resume_src = "YOUR_RESUME_URL_OR_PATH" # e.g., "[https://drive.google.com/file/d/your-resume-id/view?usp=sharing](https://drive.google.com/file/d/your-resume-id/view?usp=sharing)" or "/path/to/your/resume.pdf"
    jd_src     = "TARGET_JOB_DESCRIPTION_URL_OR_PATH" # e.g., "[https://jobs.example.com/job/123](https://jobs.example.com/job/123)" or "/path/to/job_description.txt"
    ```
4.  Run the cells sequentially to see the analysis, suggestions, and the generated cover letter.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue for bugs, feature requests, or improvements.

