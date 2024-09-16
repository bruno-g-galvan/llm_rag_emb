# AI-Based Mapping of Disease and Genetic Interactions

### **Document Loading**

-   **Input**: The project begins by loading documents (PDFs) from a directory using `PyPDFDirectoryLoader`.
-   **Purpose**: These documents contain information about diseases and genes that will later be analyzed for connections.

### **Text Splitting**
-   **Tool**: `RecursiveCharacterTextSplitter` is used to split the loaded documents into manageable chunks for easier processing.
-   **Configuration**: Each chunk has a size of 1800 characters with a 250-character overlap to ensure important information isn't lost between chunks.

### **Embedding Generation**:

-   **Model**: The project uses **Ollama embeddings** with the `llama3` model to transform the document chunks into numerical vector representations.
-   **Batch Processing**: Embeddings are created in batches of up to 1000 chunks to manage memory and performance.

### **ChromaDB Integration**:

-   **Storage**: **ChromaDB** is initialized as the vector store to persist the embeddings.
-   **Metadata Generation**: Each document chunk is assigned unique metadata (like document source, page number, and chunk index) for easy identification during retrieval.
-   **PCA (Optional)**: Though commented out in the code, you have an option to apply **Principal Component Analysis (PCA)** to reduce the dimensionality of the embeddings before storing them.

### **Database Update**:

-   **Check Existing Documents**: Before adding new chunks, the system checks for existing documents in the database by their unique IDs.
-   **Add New Chunks**: Only new documents (based on their IDs) are embedded and stored in ChromaDB.

### **Querying the Database**:

-   **LLM Query**: The LLM is prompted with a specific question (e.g., **"How can you relate Fabry disease with any other disease?"**).
-   **Similarity Search**: The system performs a similarity search in ChromaDB to find relevant chunks that best match the question.
-   **Context Building**: The top matching chunks (7 in this case) are compiled into a context for the LLM to generate a response.

### **LLM Response Generation**:

-   **Prompt Template**: A chat prompt template is used to structure the context and the question for the LLM.
-   **Response**: The **llama3** model is invoked to generate a response based on the context from ChromaDB.
-   **Source Attribution**: The response includes references to the specific document chunks used in forming the answer.

### **Output**:

-   The system outputs a formatted response containing the answer to the question and the source document metadata (IDs of the chunks).

### **Process Flow**:

-   **Documents → Text Splitter → Embeddings → ChromaDB → Query → LLM Response**.