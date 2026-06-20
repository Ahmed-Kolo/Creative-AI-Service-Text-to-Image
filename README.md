# Creative AI Service: Text-to-Image Generator

This project demonstrates a full-stack application for generating images from text prompts using a lightweight Stable Diffusion model. It leverages FastAPI for the backend API, Streamlit for an interactive web-based frontend, and Hugging Face's `diffusers` library with the `segmind/tiny-sd` model for image generation.

## Table of Contents
1. [Project Overview](#project-overview)
2. [Features](#features)
3. [Architecture](#architecture)
4. [Local Setup and Installation](#local-setup-and-installation)
5. [Running the Application](#running-the-application)
6. [Project Structure](#project-structure)
7. [Model Details & Latency](#model-details--latency)
8. [Understanding Negative Prompts](#understanding-negative-prompts)
9. [Marketing Workflow Proposal](#marketing-workflow-proposal)

## 1. Project Overview
This application provides a user-friendly interface to generate images based on textual descriptions (prompts). It's designed for rapid prototyping and demonstration of text-to-image AI capabilities, specifically tailored for scenarios requiring a FastAPI backend and a Streamlit frontend.

## 2. Features
- **Text-to-Image Generation:** Generate unique images from text prompts.
- **Negative Prompt Support:** Guide image generation away from undesired features.
- **FastAPI Backend:** Robust and scalable API for serving the image generation model.
- **Streamlit Frontend:** Interactive and intuitive user interface.
- **Containerization Ready:** Designed with separate files for easy containerization.
- **Colab Compatibility:** Demonstrates deployment and execution within a Google Colab environment.

## 3. Architecture

The application follows a client-server architecture:

- **Frontend (Streamlit `client.py`):** Provides the web interface where users input prompts and view generated images. It communicates with the FastAPI backend to request image generation.
- **Backend (FastAPI `main.py`):** Exposes a `/generate/image` API endpoint. It handles requests from the Streamlit client, loads the `segmind/tiny-sd` model (via `models.py`), generates the image, and returns it as a PNG byte stream.
- **Model Logic (`models.py`):** Encapsulates the logic for loading the `segmind/tiny-sd` model and performing image generation using the `diffusers` library.
- **Utility Functions (`utils.py`):** Contains helper functions, specifically for converting PIL Image objects to byte streams for API responses.

```mermaid
graph TD
    User --> Streamlit[Streamlit Frontend (client.py)]
    Streamlit -- HTTP GET /generate/image --> FastAPI[FastAPI Backend (main.py)]
    FastAPI --> Models[models.py (Image Generation)]
    Models -- Uses --> Diffusers[Hugging Face Diffusers]
    Models -- Loads --> TinySD[segmind/tiny-sd Model]
    Models -- Returns PIL Image --> FastAPI
    FastAPI -- Converts PIL Image to Bytes --> Utils[utils.py]
    Utils -- Returns Image Bytes --> FastAPI
    FastAPI -- Returns Image Bytes --> Streamlit
    Streamlit -- Displays Image --> User
```

## 4. Local Setup and Installation

To run this project locally, follow these steps:

1.  **Clone the Repository:**
    ```bash
    git clone <repository-url>
    cd Creative-AI-Service-Text-to-Image
    ```

2.  **Create a Virtual Environment (Recommended):**
    ```bash
    python -m venv venv
    # On macOS/Linux:
    source venv/bin/activate
    # On Windows:
    .\venv\Scripts\activate
    ```

3.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

    The `requirements.txt` file contains:
    ```
    fastapi
    uvicorn
    streamlit
    diffusers
    torch
    Pillow
    requests
    ```

## 5. Running the Application

Ensure your virtual environment is activated before running the following commands.

1.  **Start the FastAPI Backend:**
    Open your first terminal, navigate to your project directory, and run:
    ```bash
    uvicorn main:app --reload --host 0.0.0.0 --port 8000
    ```
    (The `--reload` flag is useful for development as it automatically restarts the server on code changes).

2.  **Start the Streamlit Frontend:**
    Open a **second terminal**, navigate to your project directory, and run:
    ```bash
    streamlit run client.py
    ```

3.  **Access the Streamlit App:**
    Streamlit will provide you with a local URL (usually `http://localhost:8501`) to access the application in your web browser.

    *Make sure the FastAPI backend is running before starting the Streamlit client.*

## 6. Project Structure

```
. # Project Root
├── main.py        # FastAPI backend application
├── models.py      # Contains AI model loading and inference logic
├── utils.py       # Utility functions (e.g., image to bytes conversion)
├── client.py      # Streamlit frontend application
└── requirements.txt # List of Python dependencies
```

## 7. Model Details & Latency

- **Model Used:** `segmind/tiny-sd` is a distilled version of Stable Diffusion, offering faster inference with a smaller footprint. It's ideal for applications where speed and resource efficiency are critical.
- **Inference Steps:** The `generate_image` function uses `num_inference_steps=25` to balance image quality and generation speed. This can be adjusted based on desired output quality and performance requirements.

**Latency Findings (Conceptual - requires actual measurement):**
Upon running the application locally or in a Colab environment, observe the time taken for image generation. The `main.py` backend logs the `Image generation and conversion took X.XX seconds.` message. For `segmind/tiny-sd` with 25 inference steps on typical GPU hardware (e.g., Colab T4 GPU), generation times are expected to be in the range of 1-5 seconds, significantly faster than larger Stable Diffusion models.

## 8. Understanding Negative Prompts

A **negative prompt** is an additional text input that guides the image generation process *away* from certain features or styles. While a positive prompt tells the model what you *want* to see, a negative prompt tells it what you *don't* want to see.

**Example:**
- **Positive Prompt:** "A beautiful landscape, vibrant colors, clear sky."
- **Negative Prompt:** "Blurry, low quality, grayscale, deformed, bad anatomy, text, watermark."

Using negative prompts can significantly improve the quality and relevance of generated images by preventing common artifacts or undesired elements.

## 9. Marketing Workflow Proposal

This Creative AI Service can be integrated into various marketing workflows to generate unique visual content efficiently.

**1. Content Generation for Social Media/Blogs:**
   - **Idea:** Quickly generate featured images for blog posts, social media updates, or campaign visuals.
   - **Workflow:** Content creator inputs a description of the desired image (e.g., "futuristic city skyline at sunset, cyberpunk aesthetic"). The AI generates several options, which can then be refined with negative prompts or used as inspiration for further iteration. This drastically reduces reliance on stock photos or manual graphic design for initial concepts.

**2. Rapid Prototyping for Ad Campaigns:**
   - **Idea:** Visualize different ad concepts without engaging designers for every initial idea.
   - **Workflow:** Marketing team brainstorms various ad headlines and associated visual concepts. Prompts are entered into the tool to generate visual mockups (e.g., "a happy family enjoying a new gadget, bright lighting, product in focus"). These visuals can be used for internal review, A/B testing ideas, or even for preliminary client presentations.

**3. Personalized Marketing Visuals:**
   - **Idea:** Create unique images for targeted email campaigns or personalized landing pages.
   - **Workflow:** Based on customer segmentation or preference data, generate bespoke images (e.g., "a serene beach scene with a person meditating" for a wellness product, or "a bustling urban coffee shop" for a city-centric service). This helps in making marketing messages more engaging and relevant to individual users.

**4. Mood Board Creation and Concept Exploration:**
   - **Idea:** Quickly create visual mood boards for new product launches, brand refreshes, or event themes.
   - **Workflow:** Input broad themes or keywords to generate a collection of images that convey a certain aesthetic or feeling. This aids in early-stage ideation and ensures everyone on the team is aligned on the visual direction.

**Benefits:**
- **Speed:** Generate visuals in seconds, not hours or days.
- **Cost-Effective:** Reduce expenses associated with stock photography licenses or external design services for initial concepts.
- **Creativity:** Explore a wider range of visual ideas and push creative boundaries.
- **Iterative Design:** Easily refine concepts through prompt adjustments and negative prompts.
