### EX5 - Information Retrieval Using Boolean Model in Python
### DATE: 21/04/2025

### AIM: To implement Information Retrieval Using Boolean Model in Python.

### Description:

<div align = "justify">
The Boolean model in Information Retrieval (IR) is a fundamental model used for searching and retrieving information from a collection of documents. It operates on the principles of set theory and logic, where documents are represented as sets of terms or words, and queries are expressed as Boolean expressions using logical operators such as AND, OR, and NOT.
  
### Procedure:

1. ***Initialize the BooleanRetrieval class:*** The BooleanRetrieval class is defined to manage the indexing and searching of documents.
2. ***Constructor and Index Initialization:*** The class constructor initializes an empty index to store the inverted index mapping terms to documents.
3. ***Indexing Documents:***
    <p> a) The index_document method is responsible for indexing documents.
    <p> b) Tokenize the text content of documents, converting them into lowercase terms.
    <p> c) For each term in the document, it adds an entry in the index, associating the term with the document ID. </p>
4. ***Fetch Web Page Text:***
    <p>a) The fetch_webpage_text method uses the requests library to fetch content from a given URL.
    <p>b) Extract text content from the fetched HTML using BeautifulSoup.
    <p>c) The extracted text is returned for further processing.
5. ***Boolean Search:***
    <p>a) The boolean_search method performs Boolean searches on the indexed documents.
    <p>b) Tokenize the input query and iterates through its terms.
    <p>c) For each term in the query, it retrieves documents containing that term and performs Boolean operations (AND, OR, NOT) based on the query's structure.

### Program:

```
import numpy as np
import pandas as pd

class BooleanRetrieval:
    def __init__(self):
        self.index = {}
        self.documents_matrix = None

    def index_document(self, doc_id, text):
        terms = text.lower().split()
        print("Document -", doc_id, terms)

        for term in terms:
            if term not in self.index:
                self.index[term] = set()
            self.index[term].add(doc_id)

    def create_documents_matrix(self, documents):
        terms = list(self.index.keys())
        num_docs = len(documents)
        num_terms = len(terms)

        self.documents_matrix = np.zeros((num_docs, num_terms), dtype=int)

        for i, (doc_id, text) in enumerate(documents.items()):
            doc_terms = text.lower().split()
            for term in doc_terms:
                if term in self.index:
                    term_id = terms.index(term)
                    self.documents_matrix[i, term_id] = 1

    def print_documents_matrix_table(self):
        df = pd.DataFrame(self.documents_matrix, columns=self.index.keys())
        print(df)

    def print_all_terms(self):
        print("All terms in the documents:")
        print(list(self.index.keys()))

    def boolean_search(self, query):
        query_terms = query.lower().split()
        results = set()  # Initialize as empty set to accumulate results
        current_set = None  # Current set to handle 'or' logic

        i = 0
        while i < len(query_terms):
            term = query_terms[i]

            if term == 'or':
                if current_set is not None:
                    results.update(current_set)
                current_set = None  # Reset current set for the next term
            elif term == 'and':
                i += 1
                continue  # 'and' is implicit, move to next term
            elif term == 'not':
                i += 1
                if i < len(query_terms):
                    not_term = query_terms[i]
                    if not_term in self.index:
                        not_docs = self.index[not_term]
                        if current_set is None:
                            current_set = set(range(1, len(documents) + 1))  # All doc IDs
                        current_set.difference_update(not_docs)
            else:
                if term in self.index:
                    term_docs = self.index[term]
                    if current_set is None:
                        current_set = term_docs.copy()
                    else:
                        current_set.intersection_update(term_docs)
                else:
                    current_set = set()  # If the term doesn't exist, it results in an empty set

            i += 1

        # Update results with the last processed set
        if current_set is not None:
            results.update(current_set)

        return sorted(results)

if __name__ == "__main__":
    indexer = BooleanRetrieval()

    documents = {
        1: "Python is a programming language",
        2: "Information retrieval deals with finding information",
        3: "Boolean models are used in information retrieval"
    }

    for doc_id, text in documents.items():
        indexer.index_document(doc_id, text)

    indexer.create_documents_matrix(documents)
    indexer.print_documents_matrix_table()
    indexer.print_all_terms()

    query = input("Enter your boolean query: ")
    results = indexer.boolean_search(query)
    if results:
        print(f"Results for '{query}': {results}")
    else:
        print("No results found for the query.")

```
### Output:

![Screenshot 2025-04-21 130915](https://github.com/user-attachments/assets/36869805-d1f5-46d8-9a7d-631e9e04be6c)

### Result:

Thus the implementation of Information Retrieval Using Boolean Model in Python is successfully completed.
