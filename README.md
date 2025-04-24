# MedView AI Chatbot — Backend

This is the FastAPI backend for the MedView Chatbot system. It provides smart answers to medical device questions using a mix of AI and a semantic FAQ cache. This project is built to be lightweight, fast, and easy to extend for new devices or knowledge bases.

---

## 🔧 What This Backend Does

- ✅ Answers medical device questions using either:
  - a **pre-cached FAQ** based on similarity search, or
  - a **real-time AI model** (DeepSeek/OpenAI) as a fallback.
- 🧠 Uses semantic search (FAISS + Sentence Transformers) to find relevant answers.
- ⚡ Supports **cache mode** for speed and efficiency.
- 🌐 Provides a REST API with live docs via Swagger UI.

---

## 🏁 Getting Started (Local Setup)

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/MedViewAISystem-Backend.git
cd MedViewAISystem-Backend
```

### 2. Create a Virtual Environment
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Requirements
```bash
pip install -r requirements.txt
```

### 4. Add Your `.env` File
Create a file named `.env` in the root directory with:
```
MONGO_URI=mongodb+srv://<your-mongo-uri>
```

### 5. Run the Server
```bash
uvicorn main:app --reload
```

Now visit [http://localhost:8000/docs](http://localhost:8000/docs) to test endpoints!

---

## 📁 API Overview

### `POST /asks`
Handles user-submitted questions. Uses cache if enabled, otherwise falls back to a generative model.

### `GET /faqs`
Returns 3 cached FAQs tied to the currently selected medical device.

---

## 📚 How the Caching Works

- The MongoDB database contains devices and their FAQ lists.
- User questions are embedded using `sentence-transformers` and matched to cached questions using `FAISS`.
- If the best match has a similarity score ≥ 0.7, it’s returned.
- If no match is close enough, we return a default message or pass the query to the AI model.

Example document in MongoDB:
```json
{
  "model": "AirSense 11",
  "questions": [
    {
      "question": "How do I clean it?",
      "answer": "Follow this 3-step cleaning guide..."
    },
    ...
  ]
}
```

---

## 🧰 Core Dependencies

- **FastAPI** – for creating APIs
- **PyMongo** – for MongoDB integration
- **sentence-transformers** – for text embeddings
- **FAISS** – for fast semantic search
- **python-dotenv** – for managing environment variables
- **OpenAI / DeepSeek** – for generative fallback responses

---

## 🌲 Optional: RAPTOR RAG System

Want smarter retrieval using trees? We've included a second module called RAPTOR that clusters documents and builds a semantic tree.

To run it:
```bash
cd raptor
uvicorn app:app --reload
```
More on RAPTOR here: [https://github.com/AWu497/CS370/tree/base](https://github.com/AWu497/CS370/tree/base)

---

## 🔗 Related Repositories

- **Frontend**: [MedViewAISystem-Frontend](https://github.com/your-username/MedViewAISystem-Frontend)
- **RAG Tree System**: [RAG repo](https://github.com/AWu497/CS370/tree/base)

---

## 🚀 Live Deployment

- 🧠 Backend Swagger UI: [https://medviewaisystem-backend.onrender.com/docs](https://medviewaisystem-backend.onrender.com/docs)
- 💬 Frontend App: [https://med-view-ai-system-frontend.vercel.app/](https://med-view-ai-system-frontend.vercel.app/)

---

## 💡 Tips for New Developers

- To add a new medical device:
  1. Add its FAQ document to MongoDB with the same structure.
  2. No backend code change needed unless you want custom logic.
- The frontend automatically pulls FAQ suggestions based on device selection.
- Cache mode toggles how questions are routed (backend vs AI model).
