# DeepSeek
Testing deepseek locally, it cheaper than OpenAi (about 55 cent/million input token Vs $15)
Run Deepseek locally
1. Install Ollama
- macOs/linux:  curl -fsSL https://ollama.com/install.sh | sh
- windown: downlaod from Ollama website https://ollama.com/
- check if istall was succusfull. ollama -v
2. Download DeepSeek-R1 via Ollama:
- open a terminal and run:  ollama run deepseek-r1
- there are many models: r1 is the basix model that fit with nomal GPUs
3. Open Web UI
- will run using docker. (need to install Docker)
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
4. Testing
- the application is run on: http://localhost:3000
- create or log in
- chose r1 and download the model.
5. Integrate Deepseek with projects
- Using local Deployment(for development and testing):
  + Use your Ollama instance as an OpenAI-compatible endpoint:
  
  import openai

  client = openai.Client(
      base_url="http://localhost:11434/v1",
      api_key="ollama"  # Authentication-free private access
  )

  response = client.chat.completions.create(
      model="deepseek-r1:XXb", # change the "XX" by the distilled model you choose
      messages=[{"role": "user", "content": "Explain blockchain security"}],
      temperature=0.7  # Controls creativity vs precision
  )
- Using the Official DeepSeek-R1 Cloud API. (use for production)
    + get the Deepseek api key

      import openai
      from dotenv import load_dotenv
      import os
      
      load_dotenv()
      client = openai.OpenAI(
          base_url="https://api.deepseek.com/v1",
          api_key=os.getenv("DEEPSEEK_API_KEY")
      )
      
      response = client.chat.completions.create(
          model="deepseek-reasoner",
          messages=[{"role": "user", "content": "Write web scraping code with error handling"}],
          max_tokens=1000  # Limit costs for long responses
      )
