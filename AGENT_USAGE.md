# Agent Usage Documentation

This document tracks how AI assistants were used to build this application, including the tools, prompts, delegated tasks, and error corrections.

## Tools Used
* **Primary LLM:** Groq API running `openai/gpt-oss-120b` for core application logic (extraction and mapping).
* **Code Generation Agent:** Gemini / ChatGPT for scaffolding the Next.js frontend, writing the FastAPI backend, and structuring the Tailwind UI.

## Representative Prompts
* *"Build an application that reviews a draft funding application against a supplied grant guideline... The tool must not make an authoritative legal or funding-eligibility decision."*
* *"Use groq ai api and use openai/gpt-oss-120b model... create an venv and install all the independent. also create requirements.txt."*
* *"The document can be in docx, doc or pdf or txt. and generate demo Guideline Document and Draft Application and meta data for testing."*
* *"Add this in so that i can change in vercel environment for render.com url and if there is no url then use http://localhost:8000."*

## Delegated Work
* **Backend Scaffolding:** Writing the FastAPI boilerplate, configuring CORS, setting up the Groq API client, and designing the strict JSON prompts for requirements extraction.
* **Frontend UI/UX:** Building the React state management to handle human-in-the-loop confirmations, UI styling with Tailwind CSS, and conditional rendering for the "Stale" document state.
* **Document Parsing:** Implementing `PyPDF2` and `python-docx` for handling various file formats.
* **Deployment Configuration:** Generating the `.gitignore`, `Dockerfile`, and Next.js dynamic environment variable logic.

## Important Agent Mistakes & Rejected Suggestions
* **Missing Environment Loader:** The agent initially forgot to include `python-dotenv` and the `load_dotenv()` call in the FastAPI backend, resulting in a crashed `uvicorn` server because the Groq API key was not read from the local `.env` file. (Corrected in subsequent prompt).
* **Serverless Timeout Ignorance:** The agent initially suggested hosting the Python backend on Vercel or Render's basic tier. This was rejected/modified because LLM calls frequently exceed Vercel's 10-second serverless timeout, and Render's 15-minute sleep cycle creates unacceptable cold starts. The agent was corrected to provide Dockerized alternatives like Back4app and Fly.io.

## Output Verification
* **Deterministic Logic Check:** Verified that the UI score updates strictly based on `(Confirmed Requirements / Total Mandatory Requirements) * 100`, ensuring the AI does not manipulate the final score.
* **Prompt Engineering:** Verified that the LLM returns structured JSON by using `"type": "json_object"` in the Groq API call and wrapping the system prompt in strict schema instructions.
* **File Processing:** Tested the upload mechanism locally to ensure PDF and DOCX bytes are properly decoded into strings before being passed to the LLM.