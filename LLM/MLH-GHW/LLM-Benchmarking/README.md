# LLM Benchmarking Dashboard

A Streamlit-based web application for comparing multiple Large Language Models (LLMs) side-by-side. This tool allows you to benchmark different LLMs with the same prompt and compare their performance metrics including latency, token usage, and throughput.

## 🎯 About This Project

This project provides an interactive dashboard to:
- Compare multiple LLMs simultaneously with the same prompt
- Measure and visualize performance metrics (latency, tokens, throughput)
- Get real-time side-by-side comparisons of model responses
- Analyze performance differences through interactive charts

Currently supported models:
- **Gemini 2.5 Flash** (via Google Genai API)
- **Llama 3.1 8B Instant** (via Groq API)

## 🚀 Getting Started

### Prerequisites

- Python 3.8 or higher
- Git
- API keys for:
  - Google Gemini API
  - Groq API

### Installation

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd LLM/MLH-GHW/LLM-Benchmarking
   ```

   Or if you're already in the parent directory:
   ```bash
   cd LLM/MLH-GHW/LLM-Benchmarking
   ```

2. **Create a virtual environment (recommended):**
   ```bash
   python -m venv venv
   ```

3. **Activate the virtual environment:**
   - On macOS/Linux:
     ```bash
     source venv/bin/activate
     ```
   - On Windows:
     ```bash
     venv\Scripts\activate
     ```

4. **Install dependencies:**
   ```bash
   pip install streamlit google-genai groq pandas plotly
   ```

5. **Set up API keys:**
   
   Create a `.streamlit` directory in the project root if it doesn't exist:
   ```bash
   mkdir .streamlit
   ```
   
   Create a `secrets.toml` file inside `.streamlit/`:
   ```bash
   touch .streamlit/secrets.toml
   ```
   
   Add your API keys to `.streamlit/secrets.toml`:
   ```toml
   GEMINI_API_KEY = "your-gemini-api-key-here"
   GROQ_API_KEY = "your-groq-api-key-here"
   ```

6. **Run the application:**
   ```bash
   streamlit run home.py
   ```

   The application will open in your default web browser at `http://localhost:8501`

## 📖 Usage

1. **Select Models:** Use the sidebar checkboxes to choose which models you want to compare
2. **Enter Prompt:** Type your prompt in the chat input at the bottom
3. **View Results:** 
   - See side-by-side responses from each selected model
   - Check latency and token usage for each model
   - Review the performance comparison table
   - Analyze throughput differences in the interactive chart

## 🤝 Contributing

We welcome contributions! Here's how you can help:

### How to Contribute

1. **Fork the repository**
2. **Create a feature branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes**
4. **Commit your changes:**
   ```bash
   git commit -m "Add: description of your changes"
   ```
5. **Push to your branch:**
   ```bash
   git push origin feature/your-feature-name
   ```
6. **Open a Pull Request**

### Extending the Model List

To add a new LLM model to the benchmarking dashboard:

1. **Add the model function:**
   
   Create a new function similar to `call_gemini()` or `call_llama()` in `home.py`. The function should:
   - Accept a `prompt` parameter
   - Return a tuple: `(response_text, latency, token_count)`
   - Measure the time taken for the API call
   - Extract token usage from the API response

   Example structure:
   ```python
   def call_your_model(prompt):
       start_time = time.time()
       # Your API call here
       response = your_client.generate(prompt)
       end_time = time.time()
       
       content = response.text
       token_count = response.usage.total_tokens
       
       return content, end_time - start_time, token_count
   ```

2. **Add API client initialization:**
   
   If the new model requires a different API client, initialize it at the top of the file (after the Streamlit config):
   ```python
   from your_api_library import YourClient
   
   your_client = YourClient(api_key=st.secrets["YOUR_API_KEY"])
   ```

3. **Add sidebar checkbox:**
   
   In the sidebar section, add a checkbox for your new model:
   ```python
   use_your_model = st.checkbox("Your Model Name", value=False)
   ```

4. **Add to comparison logic:**
   
   Update the comparison logic in the main prompt handling section:
   ```python
   if use_your_model:
       comparisions.append("Your Model Name")
   
   # In the results loop:
   elif comparision_name == "Your Model Name":
       content, latency, token_count = call_your_model(prompt)
   ```

5. **Update secrets.toml:**
   
   Add your API key to `.streamlit/secrets.toml`:
   ```toml
   YOUR_API_KEY = "your-api-key-here"
   ```

6. **Update dependencies:**
   
   If you added a new Python package, document it in the installation instructions or create a `requirements.txt` file.

### Contribution Guidelines

- Follow the existing code style and structure
- Add comments for complex logic
- Test your changes before submitting a PR
- Update this README if you add new features or models
- Ensure API keys are never committed to the repository (they're already in `.gitignore`)

## 📝 Project Structure

```
LLM-Benchmarking/
├── home.py              # Main Streamlit application
├── .streamlit/
│   └── secrets.toml     # API keys (not in git)
├── venv/                # Virtual environment (not in git)
└── README.md            # This file
```

## 🔒 Security Notes

- Never commit API keys or secrets to the repository
- The `.streamlit/` directory and `secrets.toml` are already in `.gitignore`
- Keep your API keys secure and rotate them if exposed

## 📄 License

[Add your license information here]

## 🙏 Acknowledgments

- Built with [Streamlit](https://streamlit.io/)
- Visualization powered by [Plotly](https://plotly.com/)
- Model APIs: Google Gemini and Groq

