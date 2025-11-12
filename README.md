# 🚀 Enterprise ATS System - Complete Setup Guide

## 📁 File Structure in VS Code

```
enterprise-ats-system/
│
├── app.py                          # Main Flask application
├── resume_analyzer.py              # Core analyzer engine (from previous artifact)
├── requirements.txt                # Python dependencies
├── README.md                       # This file
├── .gitignore                      # Git ignore file
│
├── templates/                      # HTML templates
│   ├── base.html                   # Base template with navbar
│   ├── index.html                  # Home page
│   ├── single_analysis.html        # Single resume analysis
│   ├── batch_analysis.html         # Batch analysis page
│   └── dashboard.html              # Analytics dashboard
│
├── static/                         # Static files
│   ├── css/
│   │   └── custom.css              # Custom styles (optional)
│   ├── js/
│   │   └── custom.js               # Custom JavaScript (optional)
│   └── exports/                    # Folder for exported reports
│
├── uploads/                        # Temporary file uploads
│
└── data/                          # Data storage (optional)
    └── analysis_history.json       # Persistent storage
```

---

## 🔧 Step-by-Step Setup Instructions

### **Step 1: Create Project Directory**

```bash
# Open terminal in VS Code
mkdir enterprise-ats-system
cd enterprise-ats-system
```

### **Step 2: Create Virtual Environment**

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Mac/Linux
python3 -m venv venv
source venv/bin/activate
```

### **Step 3: Create All Required Files**

#### **3.1 Create `resume_analyzer.py`**
Copy the entire code from the "Enterprise Resume Analyzer" artifact (the main Python file with the EnterpriseResumeAnalyzer class).

#### **3.2 Create `app.py`**
Copy the Flask application code from the "Flask Web Application" artifact.

#### **3.3 Create `requirements.txt`**
Copy the dependencies list from the requirements.txt artifact.

#### **3.4 Create Template Files**

```bash
# Create templates directory
mkdir templates

# Create each HTML file:
# - templates/base.html (copy from artifact)
# - templates/index.html (copy from artifact)
# - templates/single_analysis.html (copy from artifact)
# - templates/batch_analysis.html (copy from artifact)
# - templates/dashboard.html (copy from artifact)
```

#### **3.5 Create Static Directories**

```bash
mkdir static
mkdir static/css
mkdir static/js
mkdir static/exports
mkdir uploads
mkdir data
```

#### **3.6 Create `.gitignore`**

```bash
# Create .gitignore file
cat > .gitignore << EOL
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
venv/
env/
ENV/

# Flask
instance/
.webassets-cache

# Uploads
uploads/*
!uploads/.gitkeep

# Exports
static/exports/*
!static/exports/.gitkeep

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Data
data/*.json
EOL
```

---

## 📦 Step 4: Install Dependencies

### **4.1 Basic Installation (For i3 12th Gen)**

```bash
# Upgrade pip first
pip install --upgrade pip

# Install core dependencies
pip install Flask numpy pandas scikit-learn spacy

# Install document processing
pip install PyPDF2 python-docx

# Install visualization
pip install matplotlib seaborn openpyxl

# Download spaCy model
python -m spacy download en_core_web_sm
```

### **4.2 Full Installation (Including BERT & OCR)**

```bash
# Install everything from requirements.txt
pip install -r requirements.txt

# If torch installation fails (large file), use CPU-only version:
pip install torch --index-url https://download.pytorch.org/whl/cpu

# For OCR (optional, requires Tesseract installation):
# Windows: Download from https://github.com/UB-Mannheim/tesseract/wiki
# Mac: brew install tesseract
# Linux: sudo apt-get install tesseract-ocr

pip install pytesseract pdf2image
```

### **4.3 Lightweight Installation (No BERT - Faster)**

If your system is slow with BERT, use this minimal setup:

```bash
pip install Flask numpy pandas scikit-learn spacy
pip install PyPDF2 python-docx matplotlib seaborn openpyxl
python -m spacy download en_core_web_sm
```

Then in `resume_analyzer.py`, the system will automatically fall back to TF-IDF.

---

## ▶️ Step 5: Run the Application

### **Option 1: Development Mode**

```bash
# Make sure you're in the project directory and venv is activated
python app.py
```

You should see:
```
🚀 ENTERPRISE ATS SYSTEM - WEB APPLICATION
======================================================================
✅ Server starting...
📱 Access the application at: http://localhost:5000
======================================================================
```

### **Option 2: Production Mode (Using Gunicorn)**

```bash
# Install gunicorn
pip install gunicorn

# Run with gunicorn
gunicorn -w 4 -b 0.0.0.0:5000 app:app
```

### **Option 3: Windows Production (Using Waitress)**

```bash
pip install waitress
waitress-serve --port=5000 app:app
```

---

## 🌐 Step 6: Access the Application

1. **Open browser**: `http://localhost:5000`
2. **Home Page**: Overview and features
3. **Single Analysis**: Upload one resume for detailed analysis
4. **Batch Analysis**: Upload multiple resumes for comparison
5. **Dashboard**: View analytics and statistics

---

## 🧪 Testing the System

### **Test with Sample Resume**

Create a test file `test_resume.txt`:

```
John Doe
Senior Python Developer

Experience: 5 years

Skills:
- Python, Django, Flask
- PostgreSQL, MySQL
- AWS, Docker, Kubernetes
- Machine Learning, TensorFlow
- Git, CI/CD, Agile

Education:
Bachelor's in Computer Science

Work Experience:
Senior Developer at TechCorp (2020-Present)
- Built scalable web applications
- Deployed ML models in production
- Led team of 3 developers
```

### **Test Job Description**

```
Senior Python Developer - 5+ years

Required Skills:
- Python, Django or Flask
- SQL databases
- Cloud experience (AWS/Azure)
- Git version control

Nice to have:
- Machine Learning
- Docker/Kubernetes

Education: Bachelor's degree in CS or related field
```

Upload both to the Single Analysis page and verify results.

---

## 🔍 Troubleshooting

### **Issue 1: Port Already in Use**
```bash
# Windows
netstat -ano | findstr :5000
taskkill /PID <PID> /F

# Mac/Linux
lsof -ti:5000 | xargs kill -9

# Or change port in app.py:
app.run(debug=True, port=5001)
```

### **Issue 2: BERT Model Too Slow**
```python
# In resume_analyzer.py, change initialization:
analyzer = EnterpriseResumeAnalyzer(use_bert=False)
```

### **Issue 3: OCR Not Working**
```bash
# Make sure Tesseract is installed and in PATH
# Windows: Add to PATH: C:\Program Files\Tesseract-OCR
# Mac: brew install tesseract
# Linux: sudo apt-get install tesseract-ocr

# Test:
tesseract --version
```

### **Issue 4: ModuleNotFoundError**
```bash
# Make sure virtual environment is activated
# Windows: venv\Scripts\activate
# Mac/Linux: source venv/bin/activate

# Reinstall dependencies
pip install -r requirements.txt
```

### **Issue 5: Upload Size Limit**
```python
# In app.py, increase limit:
app.config['MAX_CONTENT_LENGTH'] = 50 * 1024 * 1024  # 50MB
```

---

## 🚀 Performance Optimization for i3 12th Gen

### **1. Disable BERT (Use TF-IDF)**
```python
analyzer = EnterpriseResumeAnalyzer(use_bert=False)
```

### **2. Limit Batch Size**
```python
# In app.py, add validation:
if len(files) > 50:
    return jsonify({'error': 'Maximum 50 files allowed'}), 400
```

### **3. Use Caching**
```python
from functools import lru_cache

@lru_cache(maxsize=100)
def analyze_cached(resume_hash, job_hash):
    # Cache results
    pass
```

### **4. Process in Background**
```python
from threading import Thread

def analyze_async(files, job_desc):
    # Process in background thread
    pass
```

---

## 📊 Features Overview

| Feature | Status | Description |
|---------|--------|-------------|
| 🤖 BERT Semantic Analysis | ✅ | Context-aware matching |
| 📄 PDF/DOCX/TXT Support | ✅ | Multiple formats |
| 📸 OCR for Scanned PDFs | ✅ | Extract from images |
| ⚖️ Bias Detection | ✅ | Ethical AI |
| 💡 Improvement Suggestions | ✅ | AI recommendations |
| 🎯 Confidence Scoring | ✅ | Match certainty |
| 📊 Analytics Dashboard | ✅ | Visual insights |
| 📥 Excel/JSON Export | ✅ | Data export |
| 🔄 Batch Processing | ✅ | Multiple resumes |
| 📈 Real-time Charts | ✅ | Interactive graphs |

---

## 🎓 Usage Tips

1. **Best Results**: Include complete job descriptions with required skills, experience, and qualifications
2. **Batch Analysis**: Group resumes for the same position for better comparison
3. **OCR**: Only enable for scanned PDFs (slower processing)
4. **Confidence Score**: Higher confidence = more reliable match score
5. **Bias Detection**: Review flagged resumes manually for context

---

## 🔐 Security Notes

⚠️ **For Production Deployment:**

1. Change SECRET_KEY in app.py
2. Use HTTPS (SSL certificates)
3. Add authentication (Flask-Login)
4. Implement rate limiting
5. Use database instead of in-memory storage
6. Add input validation and sanitization
7. Set up CORS properly
8. Use environment variables for config

---

## 📞 Support

For issues or questions:
1. Check troubleshooting section above
2. Review error messages in terminal
3. Enable debug mode: `app.run(debug=True)`
4. Check browser console for frontend errors

---

## 📝 License

This is a educational/demo project. Modify as needed for your use case.

---

## ✅ Quick Start Checklist

- [ ] Created virtual environment
- [ ] Installed all dependencies
- [ ] Downloaded spaCy model
- [ ] Created all directories (templates, static, uploads)
- [ ] Copied all code files
- [ ] Tested with sample resume
- [ ] Accessed web interface at localhost:5000
- [ ] Verified single analysis works
- [ ] Verified batch analysis works
- [ ] Checked dashboard displays correctly

---

🎉 **You're all set! Start analyzing resumes with AI!** 🚀