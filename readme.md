# Local Open LLM Setup

A straightforward setup for running open-source Large Language Models locally using Ollama and Open WebUI. This project provides an easy way to deploy and interact with various LLMs while maintaining full data privacy and control.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 🚀 Features

- 🔒 Complete privacy - all data stays local
- 🌐 Web-based UI for easy interaction
- 🤖 Multiple model support
- 🔧 Easy setup with Docker Compose
- 📡 API access for integration
- 💻 Cross-platform compatibility

## 📋 Prerequisites

- Docker Engine (20.10.0 or newer)
- Docker Compose (2.0.0 or newer)
- Minimum 16GB RAM (32GB recommended)
- 50GB available storage
- CPU with AVX2 support
- GPU (optional but recommended)

## 🛠️ Quick Start

1. **Clone the repository**
```bash
git clone https://github.com/randomk/local-open-llm.git
cd local-open-llm
```

2. **Start the services**
```bash
docker-compose up -d
```

3. **Access the Web UI**
- Open your browser and navigate to `http://localhost:3000`
- Follow the initial setup instructions
- Start chatting with your local LLM!

## 📦 Available Models

You can use various models including:
- Llama 2
- Mistral
- Code Llama
- Phi-2
- Neural Chat
- And many more!

### Pull a Model

Via command line:
```bash
docker exec -it ollama-ollama-1 ollama pull llama2
```

Or use the WebUI interface to download models.

## 🔧 Configuration

The setup uses a Docker Compose configuration with two main services:

```yaml
services:
  webui:
    image: ghcr.io/open-webui/open-webui:main
    ports:
      - 3000:8080/tcp
    volumes:
      - open-webui:/app/backend/data
    extra_hosts:
      - "host.docker.internal:host-gateway"
    depends_on:
      - ollama

  ollama:
    image: ollama/ollama
    expose:
      - 11434/tcp
    ports:
      - 11434:11434/tcp
    healthcheck:
      test: ollama --version || exit 1
    volumes:
      - ollama:/root/.ollama

volumes:
  ollama:
  open-webui:
```

## 🔌 API Usage

You can interact with the LLM through its API:

```python
import requests

def query_ollama(prompt, model="llama2"):
    response = requests.post('http://localhost:11434/api/generate',
        json={
            "model": model,
            "prompt": prompt
        })
    return response.json()

# Example usage
result = query_ollama("Explain quantum computing in simple terms")
print(result['response'])
```

## 📚 Documentation

For more detailed information, check out:
- [Ollama Documentation](https://github.com/ollama/ollama)
- [Open WebUI Documentation](https://github.com/open-webui/open-webui)

## 🔍 Troubleshooting

### Common Issues

1. **Memory Issues**
```bash
# Check memory usage
docker stats

# Increase system swap if needed
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

2. **Container Won't Start**
```bash
# Check logs
docker-compose logs ollama
docker-compose logs webui

# Reset containers
docker-compose down
docker-compose up -d
```

### Reset Environment
```bash
# Complete reset
docker-compose down -v
docker-compose up -d
```

## 🔒 Security Considerations

For production use:
- Change default ports
- Implement authentication
- Regular security updates
- Backup your volumes

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## ✨ Acknowledgements

- [Ollama](https://github.com/ollama/ollama)
- [Open WebUI](https://github.com/open-webui/open-webui)
- All the amazing open-source LLM developers

## 📧 Contact

Rodrigo Melgar - [Linkedin](https://www.linkedin.com/in/rodrigomelgar/))

Project Link: [https://github.com/randomk/local-open-llm](https://github.com/randomk/local-open-llm)
