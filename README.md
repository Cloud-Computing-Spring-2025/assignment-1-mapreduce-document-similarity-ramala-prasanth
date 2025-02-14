# 📌 Document Similarity Using Hadoop MapReduce

## **Objective**

The goal of this project is to compute the **Jaccard Similarity** between pairs of documents using **Hadoop MapReduce**. The Jaccard Similarity is calculated as:

$$
Jaccard Similarity (A, B) = \frac{|A \cap B|}{|A \cup B|}
$$

Where:

- `|A ∩ B|` = Number of words common in both documents
- `|A ∪ B|` = Total unique words in both documents

The output displays document pairs with their Jaccard Similarity.

---

## **👥 Example Input (input.txt)**

Each line in the input represents a document in the format:

```
<DocumentID> <DocumentContent>
```

### **Example Documents**

```
doc1 hadoop is a distributed system
doc2 hadoop is used for big data processing
doc3 big data is important for analysis
```

---

## **📤 Expected Output**

```
(doc1, doc2) 0.20
(doc1, doc3) 0.10
(doc2, doc3) 0.44
```

---

## **🛠 Approach & Implementation**

This project is implemented using **Hadoop MapReduce** with the following components:

### **1️⃣ DocumentSimilarityMapper.java** (Mapper)

- **Extracts words** from each document.
- **Emits (DocumentID, Word)** pairs.
- **Ensures unique words per document** (using a HashSet).

### **2️⃣ DocumentSimilarityReducer.java** (Reducer)

- **Builds a word set** for each document.
- **Computes Jaccard Similarity** for each document pair:
  - **Intersection** = Common words
  - **Union** = All unique words
- **Filters results to avoid zero similarity values.**

---

## **📆 Setup Instructions**

### **1️⃣ Install Hadoop**

Ensure Hadoop is installed and configured properly. If not, install it using:

```sh
sudo apt update && sudo apt install hadoop
```

Or use Docker to run Hadoop.

---

### **2️⃣ Clone the Repository**

```sh
git clone <your-repo-url>
cd <your-repo-folder>
```

---

### **3️⃣ Compile the Code**

Use Maven to build the JAR file:

```sh
mvn clean package
```

---

### **4️⃣ Upload Input Data to HDFS**

Create an input directory and upload your dataset:

```sh
hdfs dfs -mkdir -p /input
hdfs dfs -put input.txt /input/
```

Verify the file is uploaded:

```sh
hdfs dfs -ls /input/
```

---

### **5️⃣ Run the Hadoop MapReduce Job**

Execute the job using:

```sh
hadoop jar target/similarity.jar DocumentSimilarityDriver /input /output
```

---

### **6️⃣ Retrieve the Output**

Check the output stored in HDFS:

```sh
hdfs dfs -cat /output/part-r-00000
```

Download the output to your local machine:

```sh
hdfs dfs -get /output /local_output/
```

---

## **🚀 Challenges Faced & Solutions**

### **1️⃣ Issue: Jaccard Similarity Always 1.0**

- Initially, the reducer was **treating words independently** instead of comparing full document sets.
- **Solution:** We stored all words per document in a HashMap before calculating similarity.

### **2️⃣ Issue: Incorrect Union Size**

- Union was being miscalculated as `size * 2 - intersection`.
- **Solution:** Used a proper HashSet union operation to compute unique words correctly.

### **3️⃣ Issue: Mapper Emitting (Word, DocumentID) Instead of (DocumentID, Word)**

- Initially, the mapper grouped words by document incorrectly.
- **Solution:** Changed the output to `(DocumentID, Word)` for correct reducer logic.

---

## **✅ Summary**

- **Implemented Document Similarity using Hadoop MapReduce.**
- **Fixed intersection/union calculations to ensure correct Jaccard Similarity.**
- **Provided a working setup to compile, run, and retrieve results.**
- **Documented common issues and their fixes.**

---

##

