# AI-Powered Insurance Claim Analyzer

This project uses Google's Gemini AI to act as an expert insurance assistant. You can upload an insurance policy document (PDF), ask a question about a claim, and the AI will analyze the document to provide a structured answer in JSON format, indicating whether the claim should be approved or rejected and why.



---

## Core Features

* **PDF Processing**: Automatically reads and extracts all text from uploaded insurance policy PDFs.
* **Smart Document Search**: Uses advanced AI techniques (RAG) to find the *exact* clauses relevant to your query, no matter how long the document is.
* **AI-Powered Analysis**: Leverages the powerful Gemini 2.5 Pro model to understand the policy context and your query.
* **Structured Output**: Delivers a clean, predictable JSON response with a clear **Decision**, the claim **Amount**, and a **Justification** that quotes the specific policy rule.
* **Error Handling**: Includes retry mechanisms to handle potential network timeouts.

---

##  How It Works: The 5-Step Pipeline

This notebook follows a process called **Retrieval-Augmented Generation (RAG)**. Instead of just asking the AI a question from memory, we first give it the exact information it needs to answer correctly.

1.  **Extract**: The script begins by extracting all the text from the PDF you upload.
2.  **Chunk**: The text is broken down into small, overlapping "chunks." This makes it easier for the system to find specific, relevant passages.
3.  **Embed & Index**: Each chunk of text is converted into a numerical representation (an "embedding") using an open-source model. These embeddings are stored in a FAISS vector index, which is like a super-efficient library catalog for the document.
4.  **Retrieve**: When you enter your query (e.g., "knee surgery claim"), the system converts your query into an embedding and uses the FAISS index to find the most similar (i.e., most relevant) text chunks from the policy document.
5.  **Generate**: Finally, the script sends the retrieved text chunks and your original query to the Gemini AI with a specific prompt. It instructs the AI to act as an insurance expert and use *only the provided text* to make its decision and generate the final JSON output.



---

##  How to Use This Notebook

Follow these steps to get your own analysis.

### 1. Prerequisites

Before you start, you'll need a Google Gemini API key.

* Get your key from the [Google AI Studio](https://aistudio.google.com/app/apikey).
* In the Colab notebook, go to the **Secrets** tab (key icon on the left) and add a new secret named `GOOGLE_API_KEY`. Paste your key as the value.

### 2. Running the Code

1.  **Open the Notebook in Google Colab**: Upload the `.ipynb` file to your Colab environment.
2.  **Install Dependencies**: Run the first code cell (labeled ** STEP 1**) to install all the necessary libraries.
3.  **Run All Cells**: The easiest way is to go to the menu and select **Runtime > Run all**.
4.  **Upload Your PDF**: When you reach the final cell (** STEP 11**), an "Upload" button will appear. Click it and select the insurance policy PDF you want to analyze.
5.  **Enter Your Query**: After the upload is complete, an input box will appear. Type your question or claim details. For example:
    * `Is a 46-year-old male covered for knee surgery on a policy that is 3 months old?`
    * `What is the coverage for maternity expenses?`
    * `Is dental work excluded?`
6.  **Get the Result**: The script will execute the analysis pipeline and print the final JSON response from Gemini.

### Example Output

After you enter a query, you will get a response like this:

```json
{
  "Decision": "Approved",
  "Amount": "₹ 1,50,000",
  "Justification": "The policy states that 'Surgical procedures, including knee replacement surgery, are covered up to a limit of ₹ 2,00,000 after an initial waiting period of 90 days.' The 3-month-old policy meets this requirement."
}
