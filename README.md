# 📚 PDFSideKick

<div align="center">
  <img src="https://github.com/Poojan38380/pdfSidekick-frontend/public/logo-1500x300.png" alt="PDFSideKick Logo" width="200"/>
  <p><em>Your intelligent PDF companion: Upload, Search, and Chat with your documents</em></p>
  
  [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
  [![FastAPI](https://img.shields.io/badge/FastAPI-0.104.1-009688.svg?logo=fastapi)](https://fastapi.tiangolo.com/)
  [![Next.js](https://img.shields.io/badge/Next.js-15.x-black.svg?logo=next.js)](https://nextjs.org/)
  [![Python](https://img.shields.io/badge/Python-3.10+-blue.svg?logo=python)](https://www.python.org/)
  [![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue.svg?logo=typescript)](https://www.typescriptlang.org/)
</div>

---

## 🔍 Overview

PDFSideKick is a full-stack application that allows users to upload PDF documents and ask questions about their content. The application leverages advanced natural language processing to provide accurate answers based on the document content, making it easier to extract insights from lengthy PDFs without having to read through them entirely.

### 🌟 Key Features

- **📄 PDF Upload and Management**: Easily upload and organize your PDF documents
- **🔍 Smart Text Extraction**: Automatic text extraction with OCR fallback for scanned documents
- **🤖 Intelligent Q&A**: Ask questions about your documents and get accurate, contextual answers
- **⚡ Real-time Interaction**: Immediate responses through WebSocket communication
- **👁️ Processing Status Tracking**: Monitor document processing progress in real-time
- **🔒 User Authentication**: Secure access to your personal document library

### 📂 Component Repositories

This is the main repository for PDFSideKick. The frontend and backend components are available as separate repositories:

- **[Frontend Repository](https://github.com/Poojan38380/pdfSidekick-frontend)**: Next.js application with TypeScript and Tailwind CSS
- **[Backend Repository](https://github.com/Poojan38380/pdfSidekick-backend)**: FastAPI application with Python and PostgreSQL

---

## 🏗️ Architecture

PDFSideKick implements a modern, microservices-inspired architecture with a clear separation of concerns:

```
┌────────────────┐       ┌─────────────────────┐       ┌───────────────┐
│                │       │                     │       │               │
│    Frontend    │◄─────►│      Backend        │◄─────►│   Database    │
│    (Next.js)   │       │     (FastAPI)       │       │  (PostgreSQL) │
│                │       │                     │       │               │
└────────────────┘       └─────────────────────┘       └───────────────┘
                               ▲       ▲
                               │       │
                         ┌─────┘       └───────┐
                         │                     │
                 ┌───────▼────────┐     ┌──────▼─────────┐
                 │                │     │                │
                 │   Cloudinary   │     │ Language Models│
                 │  (PDF Storage) │     │  (HuggingFace) │
                 │                │     │                │
                 └────────────────┘     └────────────────┘
```

### Core Processing Flow

```
1. PDF Upload ─► 2. Text Extraction ─► 3. Text Chunking ─► 4. Embedding Generation ─► 5. Vector Storage
                                                                                        │
                                                                                        ▼
8. Answer Generation ◄─ 7. Context Retrieval ◄─ 6. Semantic Search ◄─── User Question
```

---

## 🛠️ Technology Stack

### Backend (Python/FastAPI)

- **Web Framework**: [FastAPI](https://fastapi.tiangolo.com/) - High-performance async API framework
- **Database**: PostgreSQL (via [NeonDB](https://neon.tech/)) - Serverless Postgres for vector storage
- **PDF Processing**: [PyPDF2](https://pypi.org/project/PyPDF2/) with OCR fallback
- **NLP**:
  - [Sentence Transformers](https://www.sbert.net/) (all-MiniLM-L6-v2) for embeddings
  - [Google's Flan-T5-Large](https://huggingface.co/google/flan-t5-large) for question answering
- **Real-time Communication**: FastAPI's WebSocket support
- **File Storage**: [Cloudinary](https://cloudinary.com/) for PDF document storage

### Frontend (TypeScript/Next.js)

- **Framework**: [Next.js 15.x](https://nextjs.org/) with App Router
- **Styling**: [Tailwind CSS](https://tailwindcss.com/) for responsive design
- **State Management**: React Hooks and Context API
- **WebSocket Client**: Custom implementation for real-time chat

---

## 🚀 Getting Started

### Prerequisites

- Node.js 18.x or higher
- Python 3.10 or higher
- PostgreSQL database (NeonDB recommended)
- Cloudinary account
- (Optional) HuggingFace account for API token

### Backend Setup

1. Navigate to the backend directory:

```bash
cd pdfsidekick-backend
```

2. Create and activate a virtual environment:

```bash
# Create virtual environment
python -m venv venv

# Activate on Windows
venv\Scripts\activate

# Activate on macOS/Linux
source venv/bin/activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Create a `.env` file with the following variables:

```
DATABASE_URL=your_neondb_connection_string

# Cloudinary configuration
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
CLOUDINARY_UPLOAD_PRESET=your_upload_preset

# Optional: HuggingFace API token for improved model access
HUGGINGFACEHUB_API_TOKEN=your_huggingface_token

# Optional: Custom model cache directory
MODELS_CACHE_DIR=./.model_cache
```

5. Start the backend server:

```bash
python main.py
```

The backend API will be available at `http://localhost:8000`

### Frontend Setup

1. Navigate to the frontend directory:

```bash
cd pdfsidekick-frontend
```

2. Install dependencies:

```bash
npm install
```

3. Create a `.env.local` file with:

```
NEXT_PUBLIC_BACKEND_URL=http://localhost:8000
BACKEND_URL=http://localhost:8000
```

4. Start the development server:

```bash
npm run dev
```

The frontend application will be available at `http://localhost:3000`

---

## 📋 Project Structure

### Backend Structure

```
pdfsidekick-backend/
├── api/                    # API endpoints
│   ├── __init__.py
│   └── pdfs.py             # PDF-related endpoints
├── database/               # Database functionality
│   ├── __init__.py
│   └── connection.py       # DB connection management
├── schemas/                # Pydantic schemas
│   └── __init__.py
├── utils/                  # Utility functions
│   ├── background_jobs.py  # Background processing
│   ├── cloudinary_utils.py # Cloudinary integration
│   ├── colorLogger.py      # Custom logging
│   ├── llm_client.py       # LLM integration
│   ├── local_embedding_client.py  # Embedding generation
│   ├── ocr_utils.py        # OCR capabilities
│   ├── pdf_processor.py    # PDF processing pipeline
│   └── vector_db.py        # Vector database operations
├── websocket/              # WebSocket handling
│   └── chat_server.py      # Real-time chat server
├── main.py                 # Application entry point
└── requirements.txt        # Python dependencies
```

### Frontend Structure

```
pdfsidekick-frontend/
├── app/                    # Next.js app directory
│   ├── (home)/             # Home page components
│   ├── dashboard/          # Dashboard components
│   │   ├── (sidebarOpen)/  # Dashboard with open sidebar
│   │   ├── (sidebarCollapsed)/ # Dashboard with collapsed sidebar
│   │   └── _components/    # Shared dashboard components
│   ├── hooks/              # Custom React hooks
│   ├── api/                # API client functions
│   └── lib/                # Utility libraries
├── components/             # Shared UI components
├── public/                 # Static assets
├── services/               # External service integrations
├── utils/                  # Utility functions
└── package.json            # Node.js dependencies
```

---

## 🧠 Design Philosophy & Implementation Decisions

### 1. PDF Processing Pipeline

The application implements a sophisticated PDF processing pipeline that handles both text-based and scanned documents:

#### Text Extraction Strategy

- **Primary Method**: Using PyPDF2 for efficient extraction from text-based PDFs
- **OCR Fallback**: Automatically detecting when pages have insufficient text and applying OCR
- **Chunking Approach**: Using recursive character text splitting with appropriate chunk sizes and overlaps to maintain context

#### Processing Workflow

1. **Upload**: User uploads PDF which is stored in Cloudinary
2. **Background Processing**: Asynchronous processing to avoid blocking the user interface
3. **Status Tracking**: Real-time progress updates during the processing stages
4. **Chunking**: Dividing documents into semantic units for better question answering
5. **Embedding Generation**: Converting text chunks to vector embeddings for semantic search

### 2. Semantic Search & Question Answering

The application uses a sophisticated two-stage approach to provide accurate answers:

#### Vector Search Implementation

- **Embedding Model**: Using Sentence Transformers for high-quality semantic embeddings
- **Similarity Algorithm**: Cosine similarity for efficient vector matching
- **Threshold Filtering**: Configurable relevance threshold to ensure quality
- **PostgreSQL Vector Storage**: Leveraging PostgreSQL for vector storage and retrieval

#### Answer Generation Logic

- **Context Enrichment**: Providing relevant document chunks to the LLM as context
- **Prompt Engineering**: Carefully crafted prompts to optimize answer quality
- **Model Selection**: Using Google's Flan-T5-Large for its strong zero-shot capabilities
- **Fallback Mechanisms**: Graceful handling when no relevant information is found

### 3. Real-time Communication

WebSockets enable interactive, real-time question answering:

#### WebSocket Implementation

- **Connection Management**: Per-document WebSocket connections
- **User Experience**: Immediate acknowledgment of questions
- **Progress Indicators**: "Thinking" messages during processing
- **Structured Responses**: Well-formatted JSON responses with metadata

### 4. Frontend Architecture

The frontend implements modern React patterns with Next.js:

#### UI/UX Considerations

- **Responsive Design**: Mobile-first approach with Tailwind CSS
- **Component Architecture**: Reusable, atomic components
- **Loading States**: Clear feedback during asynchronous operations
- **Error Handling**: Graceful error presentation and recovery

#### State Management Strategy

- **React Hooks**: Using hooks for component-level state
- **Context API**: For cross-component state sharing
- **Custom Hooks**: Encapsulating complex logic like WebSocket communication

---

## 🌐 API Documentation

When running the application, you can access the API documentation at:

- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

### Core Endpoints

#### PDF Management

- `POST /api/pdfs/upload` - Upload a new PDF document
- `GET /api/pdfs/user/{user_id}` - Get all PDFs for a specific user
- `GET /api/pdfs/{pdf_id}` - Get details of a specific PDF
- `GET /api/pdfs/{pdf_id}/chunks` - Get all text chunks for a specific PDF
- `GET /api/pdfs/{pdf_id}/processing-status` - Check the processing status of a PDF
- `POST /api/pdfs/{pdf_id}/reprocess` - Trigger reprocessing of a PDF
- `GET /api/pdfs/search` - Search across PDFs using semantic search

#### Real-time Chat

- `WebSocket /ws/chat/{pdf_id}` - WebSocket endpoint for real-time chat with a specific PDF

---

## 🔮 Future Enhancements

Planned future enhancements to expand the application's capabilities:

1. **📚 Multi-document Question Answering**: Allow questions that span multiple documents
2. **📝 Document Summarization**: Automatic generation of concise document summaries
3. **📷 Advanced OCR**: Improved extraction for complex documents, tables, and images
4. **👥 User Collaboration**: Shared document access and collaborative Q&A sessions
5. **📊 Advanced Analytics**: Document insights, usage statistics, and keyword extraction
6. **📱 Mobile Applications**: Native mobile clients for iOS and Android
7. **🔌 Offline Support**: Progressive Web App capabilities for offline access
8. **🎯 Custom Training**: Fine-tuning models on specific document domains
9. **🌍 Multi-language Support**: Processing documents in multiple languages
10. **🔄 Integration APIs**: APIs for third-party integration and embedding

---

## 💻 Development Considerations

### Performance Optimization

- **Model Caching**: Local caching of language models to reduce API calls
- **Chunking Strategy**: Balancing chunk size for precision vs. context retention
- **Database Indexing**: Optimized database queries for vector operations

### Scalability Planning

- **Serverless Architecture**: Using NeonDB for auto-scaling database
- **Background Processing**: Asynchronous processing to handle concurrent requests
- **Resource Management**: Careful memory management for language models

### Security Implementations

- **Authentication**: Secure user authentication and authorization
- **API Security**: Rate limiting and input validation
- **Data Protection**: Secure storage of user documents and data

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add some amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 🙏 Acknowledgements

- [FastAPI](https://fastapi.tiangolo.com/) - For the backend framework
- [Next.js](https://nextjs.org/) - For the frontend framework
- [Cloudinary](https://cloudinary.com/) - For document storage
- [HuggingFace](https://huggingface.co/) - For state-of-the-art language models
- [NeonDB](https://neon.tech/) - For PostgreSQL database hosting
- [LangChain](https://langchain.com/) - For text splitting utilities
- [Tailwind CSS](https://tailwindcss.com/) - For styling

---

<div align="center">
  <p>Built with ❤️ as part of an intern selection assignment</p>
</div>
