### **1. Project Objectives**  
**Primary Goals**:  
- Deliver **personalized** and **diverse** song recommendations using a hybrid approach (Content-Based + Collaborative Filtering).  
- Improve **user engagement** (↑ CTR, time spent) and **retention** (↑ subscriptions).  
- Solve **cold-start problems** for new songs/users.  

**Success Metrics**:  
- Achieve ≥ **80% accuracy** in recommendations (measured via user feedback/Rec@K).  
- Reduce latency to **<500ms** per recommendation.  
- Handle **10K+ concurrent users** (scalable AWS deployment).  

---

### **2. Requirements**  

#### **A. Functional Requirements**  
| ID  | Requirement | Description |  
|-----|------------|-------------|  
| FR1 | Hybrid Recommendation | Combine CB (30%) + CF (70%) with dynamic weight adjustment. |  
| FR2 | User Input | Accept `song_name` + `artist` via Streamlit UI/API. |  
| FR3 | Top-K Recommendations | Return 10 relevant songs (configurable). |  
| FR4 | Diversity Control | Allow tuning `w₁` (CB) and `w₂` (CF) via UI sliders. |  
| FR5 | Cold Start Handling | Default to CB for new songs/users. |  
| FR6 | Data Processing | Preprocess 30K songs (CB) and 97M playcounts (CF). |  

#### **B. Non-Functional Requirements**  
| ID  | Requirement | Description |  
|-----|------------|-------------|  
| NFR1 | Scalability | Support 10K RPM (AWS ASG + Dask). |  
| NFR2 | Latency | <500ms response time per recommendation. |  
| NFR3 | Data Storage | Compress sparse matrix to ≤30MB (integer encoding). |  
| NFR4 | Deployment | Zero-downtime updates (Blue/Green). |  
| NFR5 | Security | Encrypt S3 data, restrict EC2 access. |  

---

### **3. Technical Specifications**  

#### **A. Data Specifications**  
| Dataset | Format | Size | Fields |  
|---------|--------|------|--------|  
| Song Metadata | CSV/Parquet | 30K records | `track_id`, `name`, `artist`, `attributes` (tempo, genre) |  
| User Interactions | Sparse Matrix | 97M records → 30MB | `user_id`, `track_id`, `playcount` |  
| Transformed Data | TF-IDF Matrix | 30K × 8431 | Vectorized song features |  

#### **B. System Components**  
| Component | Tech Stack | Responsibility |  
|-----------|------------|----------------|  
| **Frontend** | Streamlit | User input + recommendations display |  
| **Backend** | Python (Flask/FastAPI) | Recommendation logic, API endpoints |  
| **Content-Based** | Scikit-learn (TF-IDF) | Cosine similarity on song attributes |  
| **Collaborative** | Dask, NumPy | Sparse matrix factorization (Item-Based CF) |  
| **Infra** | AWS (EC2, S3, ECR) | Hosting, storage, Docker deployment |  
| **CI/CD** | GitHub Actions, CodeDeploy | Automated testing + Blue/Green deployment |  

#### **C. APIs**  
```python  
# Example Endpoints  
POST /recommend  
Params: { "song": "Shape of You", "artist": "Ed Sheeran", "k": 10 }  
Returns: { "recommendations": [{"song": "...", "artist": "..."}, ...] }  

POST /adjust_weights  
Params: { "w1": 0.4, "w2": 0.6 }  # Update hybrid weights  
```  

---

### **4. Project Timeline (Phased Delivery)**  

| Phase | Tasks | Duration |  
|-------|-------|----------|  
| **Phase 1: Data Pipeline** | - Collect/Clean 30K songs. <br> - Preprocess user interactions (Dask). | 2 weeks |  
| **Phase 2: Algorithm Dev** | - Implement CB (TF-IDF). <br> - Implement CF (Item-Based). <br> - Hybrid integration. | 3 weeks |  
| **Phase 3: UI/API** | - Streamlit UI. <br> - FastAPI endpoints. | 2 weeks |  
| **Phase 4: Deployment** | - Dockerize app. <br> - AWS setup (EC2, S3, ECR). <br> - CI/CD pipeline. | 3 weeks |  
| **Phase 5: Testing** | - Accuracy tests (Rec@K). <br> - Load testing (Locust). | 2 weeks |  

---

### **5. Risk Mitigation**  
| Risk | Mitigation Strategy |  
|------|---------------------|  
| Sparse data for CF | Fallback to CB for low-interaction songs. |  
| High latency | Optimize Dask chunks + cache matrices. |  
| Cold starts | Use popularity-based recommendations initially. |  
| Deployment failures | Blue/Green rollback via CodeDeploy. |  

---

### **6. Stakeholders**  
- **Product Team**: Define KPIs (engagement, retention).  
- **Data Engineers**: Manage S3/Dask pipelines.  
- **DevOps**: AWS infrastructure + CI/CD.  
- **End Users**: Provide feedback via UI.  

---

### **7. Deliverables**  
1. **Code**: GitHub repo (Python, Dask, Streamlit).  
2. **Data**: Processed datasets (S3).  
3. **Deployment**: Docker image (ECR) + EC2 instances.  
4. **Documentation**: API specs, user guide, architecture diagrams.  

---


# **1. System Overview**
**Goal**:  
Build a scalable hybrid recommender system that combines **Content-Based (CB)** and **Collaborative Filtering (CF)** approaches to deliver personalized and diverse song recommendations (e.g., Spotify-like).  

**Key Metrics**:  
- User engagement (↑ CTR, time spent).  
- User retention (↑ subscription renewals).  
- Cold-start resolution for new songs/users.  

---

### **2. Architecture Diagram**
```
┌─────────────────────────────────────────────────────────────────────┐
│                           AWS Cloud Infrastructure                   │
├─────────────────┐   ┌───────────────────┐   ┌───────────────────────┤
│    S3 Bucket    │   │   ECR (Docker     │   │    EC2 Instances      │
│ (Data Storage)  │←─→│    Images)        │←─→│  (Auto Scaling Group) │
└─────────────────┘   └──────────┬────────┘   └──────────┬────────────┘
                                 │                       │
                                 ▼                       ▼
┌─────────────────────────────────────────────────────────────────────┐
│                          Application Layer                          │
├─────────────────┐   ┌───────────────────┐   ┌───────────────────────┤
│  Streamlit UI   │←─→│  Recommender      │←─→│   CI/CD Pipeline      │
│ (User Input/    │   │  Engine (Python)  │   │ (GitHub → Docker →    │
│  Recommendations)│   │                   │   │  AWS CodeDeploy)     │
└─────────────────┘   └──────────┬────────┘   └───────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│                          Data Processing Layer                       │
├─────────────────┐   ┌───────────────────┐   ┌───────────────────────┤
│  Content-Based  │←─→│  Collaborative    │←─→│   Hybrid Recommender  │
│  Filtering      │   │  Filtering (Dask) │   │  (Weighted Sum:       │
│ (TF-IDF/        │   │ (Sparse Matrix    │   │  Y = w₁×CB + w₂×CF)   │
│  Cosine Sim)    │   │  Factorization)   │   │                       │
└─────────────────┘   └──────────┬────────┘   └───────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│                          Data Storage Layer                          │
├─────────────────┐   ┌───────────────────┐   ┌───────────────────────┤
│  Song Metadata  │   │  User-Interaction │   │   Transformed         │
│ (30K songs,     │   │  Matrix (97M      │   │   Matrices (30MB)     │
│  CSV/Parquet)   │   │  records, Sparse) │   │                       │
└─────────────────┘   └───────────────────┘   └───────────────────────┘
```

---

### **3. Components Breakdown**

#### **A. Data Storage**
1. **Song Metadata (SOK Dataset)**:
   - Format: CSV/Parquet.
   - Fields: `track_id`, `song_name`, `artist`, `attributes` (genre, tempo, etc.).
   - Size: ~30K records.

2. **User Interaction Data**:
   - Raw: 97M playcount records (`user_id`, `track_id`, `playcount`).
   - Processed: Sparse matrix (60GB → compressed to 30MB via integer encoding).

3. **Transformed Data**:
   - Content-Based: TF-IDF/embeddings matrix (30K × 8431 features).
   - Collaborative: Encoded interaction matrix (Dask-friendly chunks).

#### **B. Data Processing**
1. **Content-Based Filtering**:
   - Input: Song name + artist → vectorized attributes.
   - Method: Cosine similarity on TF-IDF vectors.
   - Output: Top-K similar songs (personalized).

2. **Collaborative Filtering**:
   - Input: User-item sparse matrix.
   - Method: Item-based CF with cosine similarity (Dask for parallelization).
   - Optimization: Chunking sparse matrix (30MB per chunk).

3. **Hybrid Recommender**:
   - Weighted sum: `Y = w₁×CB_score + w₂×CF_score` (default: `w₁=0.3`, `w₂=0.7`).
   - Dynamic weights: Adjust `w₁`/`w₂` based on diversity needs.

#### **C. Application Layer**
1. **Streamlit UI**:
   - Input: User provides `song_name` + `artist`.
   - Output: Top-10 recommendations (hybrid).
   - Features: Toggle for diversity/personalization (adjusts `w₁`/`w₂`).

2. **API Endpoints** (Optional):
   - `/recommend?song=X&artist=Y&k=10`.

#### **D. Infrastructure**
1. **AWS Stack**:
   - **S3**: Stores raw/processed data (song metadata, matrices).
   - **ECR**: Docker image registry (pre-built with Python dependencies).
   - **EC2**: Hosts Streamlit app (Auto Scaling Group for load balancing).
   - **CodeDeploy**: Blue/Green deployments (zero downtime).

2. **CI/CD Pipeline**:
   - GitHub Actions → Build Docker image → Push to ECR → Deploy to EC2.
   - Tests: Pytest for recommendation logic.

3. **Data Versioning**:
   - DVC (Data Version Control) with S3 remote for tracking datasets.

---

### **4. Workflow**
1. **User Request**:
   - Submits `song_name` + `artist` via Streamlit UI.

2. **Backend Processing**:
   - **Content-Based**:
     - Fetch song vector → Compute cosine similarity → Top-K matches.
   - **Collaborative**:
     - Fetch user-item matrix → Find similar users/items → Top-K matches.
   - **Hybrid**:
     - Combine scores → Sort → Return Top-K.

3. **Dynamic Weight Adjustment**:
   - Example: Increase `w₂` (CF) if user wants more diversity.

4. **Deployment**:
   - Updates via Blue/Green:  
     - Phase 1: Deploy V0.2 to "Green" env → Test.  
     - Phase 2: Shift traffic from "Blue" (V0.1) to "Green".

---

### **5. Scalability & Optimization**
- **Data**:  
  - Use Dask for out-of-core processing (60GB → 30MB chunks).  
  - Sparse matrix formats (e.g., CSR) for memory efficiency.  
- **Compute**:  
  - Parallelize similarity computations (Dask, multiprocessing).  
- **Deployment**:  
  - ASG scales EC2 instances based on CPU/utilization.  

---

### **6. Failure Handling**
- **Cold Start**:  
  - New songs: Default to Content-Based until enough interactions.  
- **Fallback**:  
  - If CF fails (e.g., sparse data), use CB-only recommendations.  
- **Rollback**:  
  - CodeDeploy reverts to "Blue" env if errors detected.  

---

### **7. Tools & Libraries**
- **Python**: Scikit-learn (TF-IDF), Dask (CF), Streamlit (UI).  
- **AWS**: S3, EC2, ECR, CodeDeploy, ASG.  
- **DevOps**: Docker, GitHub Actions, DVC.  

---

### **8. Example Recommendation Flow**
```python
# Hybrid Recommender Pseudocode
def hybrid_recommend(song_name, artist, w1=0.3, w2=0.7):
    # Content-Based
    cb_scores = cosine_similarity(input_vector, tfidf_matrix)
    
    # Collaborative
    cf_scores = item_based_cf(track_id, interaction_matrix)
    
    # Hybrid
    hybrid_scores = (w1 * normalize(cb_scores)) + (w2 * normalize(cf_scores))
    top_k_indices = hybrid_scores.argsort()[-10:][::-1]
    
    return song_metadata.iloc[top_k_indices]
```
