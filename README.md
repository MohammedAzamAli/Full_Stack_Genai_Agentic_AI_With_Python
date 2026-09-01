# 🛡️ LLM Guardrails with Gemini & FastAPI

A practical **LLM Guardrails application** built with **FastAPI** and **Google Gemini** that demonstrates how to protect an AI application using both **input guardrails** and **output guardrails**.

The application validates a user's prompt before sending it to the Gemini model and then validates the generated response before returning it to the user.

---

## 📌 Project Overview

Large Language Models can receive unsafe prompts, prompt-injection attempts, jailbreak requests, or requests containing potentially harmful content.

This project demonstrates a simple guardrail architecture where:

1. User submits a prompt.
2. The input guardrail checks the prompt.
3. Unsafe input is blocked.
4. Safe input is sent to Google Gemini.
5. Gemini generates a response.
6. The output guardrail checks the generated response.
7. Unsafe output is blocked.
8. Safe output is returned to the user.

### 🔄 Application Flow

```text
                    ┌─────────────────┐
                    │   User Prompt   │
                    └────────┬────────┘
                             │
                             ▼
                ┌─────────────────────────┐
                │    Input Guardrails     │
                │                         │
                │ • Empty input           │
                │ • Length validation     │
                │ • Blocked keywords      │
                │ • Prompt injection      │
                │ • Jailbreak detection   │
                └────────────┬────────────┘
                             │
                     ┌───────┴───────┐
                     │               │
                   BLOCK           ALLOW
                     │               │
                     ▼               ▼
                  Response      ┌──────────────┐
                                │ Google Gemini│
                                └──────┬───────┘
                                       │
                                       ▼
                              ┌──────────────────┐
                              │ Output Guardrail │
                              │                  │
                              │ • Empty output  │
                              │ • Sensitive info│
                              └────────┬─────────┘
                                       │
                                ┌──────┴──────┐
                                │             │
                              BLOCK         ALLOW
                                │             │
                                ▼             ▼
                             Response      User Response
```

---

## ✨ Features

### 🔒 Input Guardrails

The application checks user prompts before they reach the Gemini model.

The input guardrail currently performs:

* Input type validation
* Empty prompt detection
* Maximum prompt length validation
* Blocked keyword detection
* Prompt injection detection
* Jailbreak detection

The maximum prompt length is currently **5,000 characters**.

---

### 🚫 Blocked Keywords

The application checks for potentially harmful keywords such as:

* `hack`
* `malware`
* `ransomware`
* `phishing`
* `bomb`
* `terrorist`
* `terrorism`
* `kill`
* `murder`
* `weapon`

The keyword matching uses regular expressions with word boundaries.

---

### 🧠 Prompt Injection Detection

The application checks for common prompt-injection patterns, including attempts to:

* Ignore previous instructions
* Override system instructions
* Reveal system prompts
* Reveal instructions
* Access developer instructions

Example categories include:

```text
PROMPT_INJECTION
```

---

### 🔓 Jailbreak Detection

The application also checks for jailbreak-related patterns such as:

```text
jailbreak
jailbreak mode
do anything now
DAN mode
developer mode
god mode
unrestricted mode
uncensored mode
bypass safety
bypass safeguards
disable safety
ignore restrictions
```

If a jailbreak pattern is detected, the request is blocked.

---

## 🛡️ Output Guardrails

Input validation alone is not enough.

Even after a prompt passes the input guardrail, the generated response is checked before it is returned to the user.

The output guardrail currently checks for:

* Empty model responses
* Potentially sensitive information

Blocked output patterns include:

```text
api key
api_key
secret key
password
private key
```

If potentially sensitive information is detected, the response is blocked.

---

## 🤖 Google Gemini Integration

The project uses the Google Gen AI Python SDK to communicate with the Gemini model.

The Gemini client is initialized using an API key stored in an environment variable.

The application sends the user's validated prompt to Gemini and receives the generated response.

---

## ⚙️ Technologies Used

| Technology          | Purpose                         |
| ------------------- | ------------------------------- |
| Python              | Core programming language       |
| FastAPI             | Backend API framework           |
| Uvicorn             | ASGI server                     |
| Google Gemini       | Large Language Model            |
| Google Gen AI SDK   | Gemini API integration          |
| Pydantic            | Request validation              |
| Jinja2              | HTML template rendering         |
| python-dotenv       | Environment variable management |
| HTML/CSS/JavaScript | Frontend interface              |

---

## 📂 Project Structure

```text
Full_Stack_Genai_Agentic_AI_With_Python/
│
├── static/
│   └── Frontend static files
│
├── templates/
│   └── index.html
│
├── app.py
├── config.py
├── gemini_service.py
├── guardralis.py
├── output_guardralis.py
├── requirements.txt
├── .gitignore
└── README.md
```

### File Description

#### `app.py`

Main FastAPI application.

It contains:

* FastAPI configuration
* Static file configuration
* Jinja2 templates
* Request model
* Home page
* Health-check endpoint
* `/generate` API endpoint
* Input guardrail integration
* Gemini integration
* Output guardrail integration

---

#### `guardralis.py`

Contains the **input guardrail system**.

It is responsible for checking:

```text
Invalid Input
      ↓
Empty Input
      ↓
Input Length
      ↓
Blocked Keywords
      ↓
Prompt Injection
      ↓
Jailbreak
      ↓
SAFE
```

---

#### `output_guardralis.py`

Contains the **output guardrail system**.

It validates Gemini's response before returning it to the user.

---

#### `gemini_service.py`

Handles communication with Google Gemini.

The main function is:

```python
generate_response(prompt)
```

---

#### `config.py`

Loads environment variables and retrieves:

```text
GEMINI_API_KEY
GEMINI_MODEL
```

---

#### `requirements.txt`

Contains the Python dependencies required by the project.

---

## 🔑 Environment Variables

Create a `.env` file in the project root:

```env
GEMINI_API_KEY=your_gemini_api_key
GEMINI_MODEL=your_gemini_model
```

Do **not** upload your `.env` file or API key to GitHub.

The project uses `python-dotenv` to load environment variables.

---

## 🚀 Installation

### 1. Clone the repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
```

### 2. Navigate into the project

```bash
cd Full_Stack_Genai_Agentic_AI_With_Python
```

### 3. Create a virtual environment

Windows:

```powershell
python -m venv venv
```

### 4. Activate the virtual environment

```powershell
venv\Scripts\activate
```

### 5. Install dependencies

```powershell
pip install -r requirements.txt
```

### 6. Create `.env`

```env
GEMINI_API_KEY=your_api_key
GEMINI_MODEL=your_model
```

### 7. Run the application

```powershell
uvicorn app:app --reload
```

The application will be available at:

```text
http://127.0.0.1:8000
```

---

## 🔌 API Endpoints

### `GET /`

Displays the application's web interface.

---

### `GET /health`

Health-check endpoint.

Example response:

```json
{
  "status": "healthy",
  "application": "LLM Guardrails Workshop"
}
```

---

### `POST /generate`

Generates an AI response after applying the guardrails.

Request:

```json
{
  "prompt": "Explain what an AI agent is."
}
```

### Successful response

```json
{
  "status": "success",
  "stage": "completed",
  "category": "SAFE",
  "message": "Response generated successfully.",
  "response": "..."
}
```

---

## 🚨 Blocked Input Response

If the input guardrail detects an unsafe request, the application returns a response similar to:

```json
{
  "status": "blocked",
  "stage": "input_guardrail",
  "category": "PROMPT_INJECTION",
  "message": "Prompt injection detected.",
  "response": null
}
```

---

## 🚨 Blocked Output Response

If Gemini generates potentially sensitive content, the output guardrail can block the response:

```json
{
  "status": "blocked",
  "stage": "output_guardrail",
  "category": "SENSITIVE_OUTPUT",
  "message": "Potentially sensitive information detected in model output.",
  "response": null
}
```

---

## 🔄 Complete Request Lifecycle

```text
User
 │
 ▼
FastAPI
 │
 ▼
Input Guardrail
 │
 ├── ❌ Unsafe → Block
 │
 └── ✅ Safe
       │
       ▼
   Gemini Model
       │
       ▼
Output Guardrail
 │
 ├── ❌ Unsafe → Block
 │
 └── ✅ Safe
       │
       ▼
    Response
       │
       ▼
      User
```

---

## 🎯 Purpose of the Project

The main purpose of this project is to demonstrate the concept of **LLM safety and guardrails** in a practical application.

It shows how developers can place security and validation layers around an LLM instead of directly passing every user request to the model.

---

## 🚀 Future Improvements

Possible improvements include:

* More advanced semantic safety detection
* Better prompt-injection detection
* Context-aware guardrails
* Toxicity detection
* Personally identifiable information (PII) detection
* Rate limiting
* Authentication
* Request logging
* Database integration
* Automated guardrail testing
* More sophisticated output validation
* Production-grade monitoring
* Docker deployment
* Cloud deployment

---

## ⚠️ Disclaimer

This project demonstrates a **basic rule-based guardrail implementation for educational purposes**.

Keyword and pattern matching alone cannot detect every unsafe request or every unsafe model response. A production application should use more comprehensive security, validation, monitoring, and testing mechanisms.

---

## 👨‍💻 Author

**PRAKASH SENAPATI**

LLM Guardrails Workshop Project

---

## ⭐ If You Find This Project Useful

Consider giving the repository a ⭐ on GitHub and exploring the project to understand how LLM guardrails can be implemented around a generative AI application.
