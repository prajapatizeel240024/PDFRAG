# PDF Conversational AI with LangChain and FAISS

![image](https://github.com/user-attachments/assets/e0985bb6-83fd-441c-a56a-e4fe7b03b917)


## **Overview**
This project provides an AI-powered conversational system to interact with PDF documents. Leveraging OpenAI's GPT model, **LangChain** for conversational retrieval, and **FAISS** as a vector store for embeddings, this application enables users to upload a PDF, extract its content, and engage in an interactive Q&A session via a streamlined **Streamlit** interface.

## **Features**
- Extract text from uploaded PDF files.
- Split text into manageable chunks for efficient processing.
- Generate vector embeddings and store them in a FAISS database for retrieval.
- Create a conversational retrieval system for interactive Q&A with the document.
- User-friendly chat interface with customizable templates.

---

## **Requirements**
- Python 3.7 or higher.
- OpenAI API key (stored in a `.env` file).

---

## **Installation and Setup**

1. **Set Up Environment Variables**:
   - Create a `.env` file in the root directory of your project.
   - Add your OpenAI API key to the file:
     ```plaintext
     OPENAI_API_KEY=your_openai_api_key
     ```

2. **Install Dependencies**:
   Install the required Python packages by running:
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the HTML Templates Setup**:
   Before launching the main application, set up the chat templates by executing:
   ```bash
   python htmlTemplates.py
   ```

4. **Run the Streamlit Application**:
   Start the application using Streamlit:
   ```bash
   streamlit run app.py
   ```

---

## **Usage**

### **Step 1: Upload a PDF**
- In the Streamlit app, use the file uploader to upload a PDF document.

### **Step 2: Processing the PDF**
- The system extracts text from the uploaded PDF and splits it into chunks for efficient retrieval.

### **Step 3: Interactive Chat**
- Once processing is complete, you can interact with the PDF content by typing questions in the chat box.
- Example:
  ```plaintext
  You: What is the main topic of the document?
  Assistant: The document primarily discusses the principles of AI and its applications.
  ```

### **Step 4: Exit**
- To stop the chat session, type `exit`.

---

## **Project Architecture**

1. **PDF Upload and Text Extraction**:
   - The app reads and extracts text from uploaded PDF files using `PyPDF2`.

2. **Text Chunking**:
   - The extracted text is split into smaller chunks using `LangChain`'s `CharacterTextSplitter`.

3. **Embeddings Generation**:
   - The text chunks are embedded into vector space using `OpenAIEmbeddings`.

4. **FAISS Vector Store**:
   - The embeddings are stored in a FAISS database for quick and efficient retrieval.

5. **Conversational Retrieval Chain**:
   - A LangChain-based conversational retrieval chain connects the vector database to the chatbot interface.

6. **Streamlit Chat Interface**:
   - Users interact with the chatbot through a web-based interface powered by Streamlit.

---

## **Project Structure**

```plaintext
.
├── app.py                # Main Streamlit application
├── htmlTemplates.py      # Contains the CSS and HTML templates for the chat interface
├── requirements.txt      # List of dependencies
├── README.md             # Documentation
├── .env                  # API key for OpenAI (not included in the repository)
```

---

## **Features and Customizations**

1. **Temperature Setting**:
   - The chatbot's creativity can be adjusted by modifying the `temperature` parameter in the `app.py`.

2. **Chunk Size and Overlap**:
   - Adjust `chunk_size` and `chunk_overlap` in the `CharacterTextSplitter` to handle documents of varying lengths.

3. **CSS and HTML Customization**:
   - Modify `htmlTemplates.py` for personalized chatbot styles.

---

## **Example Workflow**

### Uploading and Interacting with a PDF

1. **Upload**:
   - Drag and drop a PDF or select one using the file uploader in the Streamlit interface.

2. **Interact**:
   - Ask questions such as:
     ```plaintext
     You: What is the primary focus of the document?
     Assistant: The document is focused on the implementation of AI in education.
     ```

3. **Exit**:
   - To stop the interaction, simply type `exit`.

---

## **Dependencies**

Add the following to your `requirements.txt` file:
```plaintext
streamlit
python-dotenv
PyPDF2
langchain
openai
faiss-cpu
```

Install all dependencies using:
```bash
pip install -r requirements.txt
```

---

## **License**
This project is licensed under the MIT License.

---

## **Acknowledgments**
- **OpenAI**: For GPT-powered conversational AI.
- **LangChain**: For providing tools for conversational retrieval.
- **FAISS**: For efficient vector-based storage and retrieval.
- **Streamlit**: For an easy-to-use interactive interface.


# News/Articles System - Backend Explanation for Frontend

## 📋 Overview

The backend automatically fetches immigration news articles from 5 RSS feeds, categorizes them by visa type, and provides personalized news based on user's visa applications.

---

## 🔄 How It Works

### 1. **Automatic News Fetching** (Background Process)

**What happens:**
- Backend fetches articles from 5 immigration news sources every 6 hours
- Sources: CitizenPath, Immigration Impact, VisaPro, Murthy, Immigration.com
- Each article is automatically categorized by visa type using keyword matching

**Auto-Categorization:**
- System scans article title + content for keywords
- Matches keywords to visa categories (E1, E2, F1, Citizenship, etc.)
- If no match found → categorizes as "General"
- Articles can have multiple categories

**Example:**
```
Article: "E-1 Visa Processing Updates for 2025"
Keywords found: "e-1", "e1", "treaty trader"
→ Categorized as: ["E1"]
```

---

## 📡 API Endpoints

### Base URL: `/api/v1/news`

### 1. **Get All News** (Public)
```
GET /api/v1/news
```

**Query Parameters:**
- `page` (optional, default: 1) - Page number
- `limit` (optional, default: 10) - Items per page
- `source` (optional) - Filter by source: "citizenpath", "immigration-impact", "visapro", "murthy", "immigration-com"
- `category` (optional) - Filter by single category: "E1", "F1", "Citizenship", etc.
- `categories` (optional) - Filter by multiple categories (comma-separated): "E1,F1,TN"
- `search` (optional) - Search in title/content

**Response:**
```json
{
  "success": true,
  "data": {
    "news": [
      {
        "_id": "507f1f77bcf86cd799439011",
        "title": "E-1 Visa Processing Updates",
        "link": "https://example.com/article",
        "pubDate": "2025-01-15T10:00:00Z",
        "summary": "Recent changes to E-1 visa processing...",
        "source": "citizenpath",
        "guid": "unique-article-id",
        "categories": ["E1"],
        "imageUrl": "https://example.com/image.jpg",
        "isActive": true,
        "createdAt": "2025-01-15T10:00:00Z",
        "updatedAt": "2025-01-15T10:00:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 150,
      "totalPages": 15
    }
  }
}
```

**Example Usage:**
```javascript
// Get all E-1 visa news
GET /api/v1/news?category=E1&page=1&limit=10

// Get news from multiple categories
GET /api/v1/news?categories=E1,F1,TN&page=1

// Search for articles
GET /api/v1/news?search=visa+processing
```

---

### 2. **Get Personalized News** (Protected - Requires Auth)
```
GET /api/v1/news/personalized
Headers: Authorization: Bearer <token>
```

**How it works:**
1. Backend looks at user's orders (ACTIVE, PREPARING, SUBMITTED, COMPLETED)
2. Extracts service slugs from orders (e.g., "e1-visa", "f1-student-visa")
3. Maps service slugs to news categories:
   - "e1-visa" → "E1"
   - "f1-student-visa" → "F1"
   - "citizenship-naturalization" → "Citizenship"
4. Returns news articles matching those categories

**Query Parameters:**
- `page` (optional, default: 1)
- `limit` (optional, default: 10)

**Response:**
```json
{
  "success": true,
  "data": {
    "news": [...],
    "matchedCategories": ["E1", "F1"],  // Which categories matched user's visas
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 25,
      "totalPages": 3
    }
  }
}
```

**Example:**
```
User has orders for:
- E-1 Visa service
- F-1 Student Visa service

Backend returns:
- All E-1 related news
- All F-1 related news
- Shows which categories matched: ["E1", "F1"]
```

**If user has no orders:**
- Returns "General" category news

---

### 3. **Get Latest News** (Public)
```
GET /api/v1/news/latest?limit=5&category=E1
```

**Query Parameters:**
- `limit` (optional, default: 5) - Number of articles
- `category` (optional) - Filter by category

**Use Case:** Dashboard widgets, homepage featured articles

---

### 4. **Get Single Article** (Public)
```
GET /api/v1/news/:id
```

**Response:** Full article with content field included

---

### 5. **Get Categories** (Public)
```
GET /api/v1/news/categories
```

**Response:**
```json
{
  "success": true,
  "data": [
    "E1", "E2", "E3", "H1B", "H1B-Non-Profit", "J1", "M1", "F1", "TN",
    "Adjustment-of-Status", "Family-Petition", "Green-Card-Renewal",
    "Removal-of-Conditions", "Citizenship", "Asylum", "TPS", "General"
  ]
}
```

---

### 6. **Get Sources** (Public)
```
GET /api/v1/news/sources
```

**Response:**
```json
{
  "success": true,
  "data": [
    "citizenpath",
    "immigration-impact",
    "visapro",
    "murthy",
    "immigration-com"
  ]
}
```

---

### 7. **Get Category Statistics** (Public)
```
GET /api/v1/news/stats
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "category": "E1",
      "count": 45,
      "latestDate": "2025-01-15T10:00:00Z"
    },
    {
      "category": "F1",
      "count": 32,
      "latestDate": "2025-01-14T08:00:00Z"
    }
  ]
}
```

---

## 📊 Data Structure

### Article Object
```typescript
{
  _id: string;                    // MongoDB ObjectId
  title: string;                  // Article title
  link: string;                   // Original article URL
  pubDate: Date;                  // Publication date
  content?: string;               // Full content (only in /:id endpoint)
  summary?: string;              // Auto-generated summary (200 chars max)
  source: string;                // "citizenpath" | "immigration-impact" | "visapro" | "murthy" | "immigration-com"
  guid: string;                  // Unique identifier (prevents duplicates)
  categories: string[];           // Visa categories: ["E1", "F1"] or ["General"]
  imageUrl?: string;             // Article image URL
  isActive: boolean;             // Whether article is active
  createdAt: Date;               // When added to database
  updatedAt: Date;               // Last update time
}
```

**Note:** `content` field is excluded from list endpoints (only in single article endpoint)

---

## 🎯 Visa Categories

Available categories:
- `E1` - E-1 Treaty Trader Visa
- `E2` - E-2 Treaty Investor Visa
- `E3` - E-3 Australian Professional Visa
- `H1B` - H-1B Specialty Occupation
- `H1B-Non-Profit` - H-1B Cap-Exempt
- `J1` - J-1 Exchange Visitor
- `M1` - M-1 Vocational Student
- `F1` - F-1 Student Visa
- `TN` - TN NAFTA/USMCA Visa
- `Adjustment-of-Status` - AOS (I-485)
- `Family-Petition` - Family Petition (I-130)
- `Green-Card-Renewal` - Green Card Renewal (I-90)
- `Removal-of-Conditions` - Removal of Conditions (I-751)
- `Citizenship` - Citizenship/Naturalization (N-400)
- `Asylum` - Asylum
- `TPS` - Temporary Protected Status
- `General` - General immigration news

---

## 🔐 Authentication

**Public Endpoints** (No auth required):
- `GET /api/v1/news`
- `GET /api/v1/news/latest`
- `GET /api/v1/news/categories`
- `GET /api/v1/news/sources`
- `GET /api/v1/news/stats`
- `GET /api/v1/news/:id`

**Protected Endpoints** (Auth required):
- `GET /api/v1/news/personalized` - Requires Bearer token

**How to authenticate:**
```javascript
Headers: {
  "Authorization": "Bearer <firebase_token>"
}
```

---

## 💡 Frontend Implementation Examples

### Example 1: Get Personalized News for Logged-in User
```javascript
// Frontend code
const getPersonalizedNews = async (page = 1, limit = 10) => {
  const response = await fetch(
    `/api/v1/news/personalized?page=${page}&limit=${limit}`,
    {
      headers: {
        'Authorization': `Bearer ${userToken}`
      }
    }
  );
  const data = await response.json();
  
  // data.data.news - Array of articles
  // data.data.matchedCategories - Which visa categories matched
  // data.data.pagination - Pagination info
  return data;
};
```

### Example 2: Get News by Category
```javascript
const getNewsByCategory = async (category, page = 1) => {
  const response = await fetch(
    `/api/v1/news?category=${category}&page=${page}&limit=10`
  );
  return await response.json();
};

// Usage
const e1News = await getNewsByCategory('E1');
const f1News = await getNewsByCategory('F1');
```

### Example 3: Get Latest News for Dashboard
```javascript
const getLatestNews = async (category = null) => {
  const url = category 
    ? `/api/v1/news/latest?limit=5&category=${category}`
    : `/api/v1/news/latest?limit=5`;
    
  const response = await fetch(url);
  return await response.json();
};
```

### Example 4: Search News
```javascript
const searchNews = async (searchTerm, page = 1) => {
  const response = await fetch(
    `/api/v1/news?search=${encodeURIComponent(searchTerm)}&page=${page}`
  );
  return await response.json();
};
```

---

## 🔄 Service Slug to Category Mapping

When user has orders, backend maps service slugs to categories:

| Service Slug | News Category |
|--------------|---------------|
| `e1-visa` | `E1` |
| `e1-visa-renewal` | `E1` |
| `e2-visa` | `E2` |
| `e2-visa-renewal` | `E2` |
| `e3-visa` | `E3` |
| `e3-visa-renewal` | `E3` |
| `tn` | `TN` |
| `f1-student-visa` | `F1` |
| `j1-exchange-visitor` | `J1` |
| `m1-vocational-visa` | `M1` |
| `adjustment-of-status` | `Adjustment-of-Status` |
| `family-petition` | `Family-Petition` |
| `green-card-renewal-replacement` | `Green-Card-Renewal` |
| `removal-of-conditions` | `Removal-of-Conditions` |
| `citizenship-naturalization` | `Citizenship` |
| `fiance-visa` | `Family-Petition` |

---

## ⚡ Performance Notes

1. **Pagination:** Always use pagination for large lists (default: 10 items per page)
2. **Caching:** Consider caching category lists and stats (they don't change often)
3. **Indexes:** Backend has indexes on `pubDate`, `categories`, `source`, `isActive` for fast queries
4. **Content Field:** Excluded from list endpoints for faster responses (only in single article endpoint)

---

## 🚨 Error Handling

**401 Unauthorized:**
```json
{
  "success": false,
  "error": "Authentication required"
}
```

**404 Not Found:**
```json
{
  "success": false,
  "error": "News not found"
}
```

**500 Internal Server Error:**
```json
{
  "success": false,
  "error": "Internal server error message"
}
```

---

## 📝 Summary

1. **Backend automatically fetches** news from 5 RSS feeds every 6 hours
2. **Auto-categorizes** articles by visa type using keyword matching
3. **Personalized news** based on user's visa orders (requires auth)
4. **Public endpoints** for browsing all news, filtering by category, searching
5. **Protected endpoint** for personalized news based on user's visa types
6. **Pagination** supported on all list endpoints
7. **Content field** only included in single article endpoint (excluded from lists for performance)

---

## 🎨 Frontend UI Suggestions

1. **Homepage:** Show latest news (5-10 articles)
2. **News Page:** Full list with filters (category, source, search)
3. **User Dashboard:** Personalized news widget (if logged in)
4. **Category Pages:** Filter by specific visa type
5. **Article Detail:** Full article view with link to original source

---

## ❓ Questions?

If frontend needs clarification on:
- Response formats
- Error handling
- Authentication
- Category mappings
- Pagination

Contact backend team!


