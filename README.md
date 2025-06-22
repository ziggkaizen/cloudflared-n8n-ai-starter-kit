<!--
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at:
    http://www.apache.org/licenses/LICENSE-2.0

This file is part of the Self-hosted AI Starter Kit by n8n.io.
Modified by Zigg Kaizen on 2025.03.24: Added Cloudflared and Nginx support.
-->

# Cloudflared N8N - Self-hosted AI starter kit

**Self-hosted AI Starter Kit** is an open-source Docker Compose template designed to swiftly initialize a comprehensive local AI and low-code development environment.
Curated by <https://github.com/n8n-io>, it combines the self-hosted n8n platform with a curated list of compatible AI products and components to quickly get started with building self-hosted AI workflows.

The **Cloudflared N8N AI Starter Kit (This fork)** builds on the original Self-hosted AI Starter Kit by integrating Cloudflared and an Nginx reverse proxy. This enhancement allows you to deploy your local n8n instance behind a secure reverse proxy using your own domain. With this setup, your locally installed n8n becomes accessible from the internet while maintaining the benefits of local hosting, such as improved data privacy and control. The integration provides a robust and flexible solution for securely managing and accessing your AI workflows.

Additionally it comes with a preinstalled workflow for taking backups of your workflows and credentioals, locally saved as json files. Hope you find this helpfull. Cheers!

![n8n.io - Screenshot](https://raw.githubusercontent.com/ziggkaizen/cloudflared-n8n-ai-starter-kit/main/assets/n8n-demo.gif)

---

## Fork Notice & Modifications

This repository is a fork of the original [Self-hosted AI Starter Kit by n8n.io](https://github.com/n8n-io/self-hosted-ai-starter-kit) and is distributed under the Apache License 2.0.  
**Modifications Made:**  
- Added support for Cloudflared to establish secure tunnels.
- Integrated Nginx as a reverse proxy with SSL termination.
- Updated Docker Compose configurations to support the above additions.
- Delivered with a preinstalled workflow to take local backups of your workflows and credentials.

---

### What’s included

✅ [**Self-hosted n8n**](https://n8n.io/) - Low-code platform with over 400
integrations and advanced AI components

✅ [**Ollama**](https://ollama.com/) - Cross-platform LLM platform to install
and run the latest local LLMs

✅ [**Qdrant**](https://qdrant.tech/) - Open-source, high performance vector
store with an comprehensive API

✅ [**PostgreSQL**](https://www.postgresql.org/) -  Workhorse of the Data
Engineering world, handles large amounts of data safely.

✅ [**Cloudflared**](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) – Secure tunnel to connect a domain to your local n8n install.

✅ [**Nginx**](https://nginx.org/) – As reverse proxy and SSL termination.


### What you can build

⭐️ **AI Agents** for scheduling appointments

⭐️ **Summarize Company PDFs** securely without data leaks

⭐️ **Smarter Slack Bots** for enhanced company communications and IT operations

⭐️ **Private Financial Document Analysis** at minimal cost

## Installation

### Prerequisites

1. A registered domain of your own.
2. A Cloudflare account (free sign-up available at <https://dash.cloudflare.com/sign-up>).
3. Your domain must be added to your Cloudflare account.
4. Your domain must be configured with Cloudflare DNS.

### Setting up your tunnel

Follow the steps below or follow this detailed guide to set up your tunnel:

[How to set up a cloudflared tunnel - Google Docs](https://docs.google.com/document/d/1lIIS_uWS5SR4RGdggBQ0fXtszF7SdgbzqdngbgTR0Gc/edit?usp=sharing)

 1. Sign in to your Cloudflare account at [cloudflare.com/login](https://cloudflare.com/login).  
 2. In the left-hand menu, click on **Zero Trust**.  
 3. Within the Zero Trust dashboard, navigate to **Network → Tunnels** and then select **Add a tunnel**.  
 4. Click on **Select Cloudflared**.  
 5. Enter a name of your choice for the tunnel.  
 6. Click the **Save tunnel** button.  
 7. Choose any environment and copy the terminal command that includes your personal token.  
 8. Extract the token from the command and store it securely. Do not share this token with anyone. (For example, you should see a token similar to the one in this Docker command:  
   ```
   docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token eyJhIjoiNDNlMjVmYjcxODNkNDZhZjRmYzdiYzExOWU3YmVmZWEiLCJ0IjoiNzM5NDE2YjI0OTNlYTFlZmI4MjZmODhlZjY3MWRhNGQiLCJzIjoiTWpReU1qVXdPRFl4TWpReU56Z3pNRGcxTWpneSIsImIiOiJwYXlsb2FkIn0=
   ```  
   )  
 9. Finally, click the **Next** button.
10. Finally set up your traffic routing.

Get the full 

### Cloning the Repository

```bash
git clone https://github.com/ziggkaizen/cloudflared-n8n-ai-starter-kit.git
cd self-hosted-ai-starter-kit
```

### Running n8n using Docker Compose

#### For Nvidia GPU users

```
git clone https://github.com/ziggkaizen/cloudflared-n8n-ai-starter-kit.git
cd cloudflared-n8n-ai-starter-kit
docker compose --profile gpu-nvidia up -d
```

> [!NOTE]
> If you have not used your Nvidia GPU with Docker before, please follow the
> [Ollama Docker instructions](https://github.com/ollama/ollama/blob/main/docs/docker.md).

### For AMD GPU users on Linux

```
git clone https://github.com/ziggkaizen/cloudflared-n8n-ai-starter-kit.git
cd cloudflared-n8n-ai-starter-kit
docker compose --profile gpu-amd up -d
```

#### For Mac / Apple Silicon users

If you’re using a Mac with an M1 or newer processor, you can't expose your GPU
to the Docker instance, unfortunately. There are two options in this case:

1. Run the starter kit fully on CPU, like in the section "For everyone else"
   below
2. Run Ollama on your Mac for faster inference, and connect to that from the
   n8n instance

If you want to run Ollama on your mac, check the
[Ollama homepage](https://ollama.com/)
for installation instructions, and run the starter kit as follows:

```
git clone https://github.com/ziggkaizen/cloudflared-n8n-ai-starter-kit.git
cd cloudflared-n8n-ai-starter-kit
docker compose up -d
```

##### For Mac users running OLLAMA locally

If you're running OLLAMA locally on your Mac (not in Docker), you need to modify the OLLAMA_HOST environment variable
in the n8n service configuration. Update the x-n8n section in your Docker Compose file as follows:

```yaml
x-n8n: &service-n8n
  # ... other configurations ...
  environment:
    # ... other environment variables ...
    - OLLAMA_HOST=host.docker.internal:11434
```

Additionally, after you see "Editor is now accessible via: <https://subdomain.example.com>":

1. Head to <https://subdomain.example.com/home/credentials>
2. Click on "Local Ollama service"
3. Change the base URL to "http://host.docker.internal:11434/"

#### For everyone else

```
git clone https://github.com/ziggkaizen/cloudflared-n8n-ai-starter-kit.git
cd cloudflared-n8n-ai-starter-kit
docker compose --profile cpu up -d
```

## ⚡️ Quick start and usage

The core of the Self-hosted AI Starter Kit is a Docker Compose file, pre-configured with network and storage settings, minimizing the need for additional installations.
After completing the installation steps above, simply follow the steps below to get started.

1. Open <https://subdomain.example.com/> (tour domain) in your browser to set up n8n. You’ll only
   have to do this once.
2. Open the included workflow:
   <https://subdomain.example.com/workflow/srOnR8PAY3u4RSwb>
3. Click the **Chat** button at the bottom of the canvas, to start running the workflow.
4. If this is the first time you’re running the workflow, you may need to wait
   until Ollama finishes downloading Llama3.2. You can inspect the docker
   console logs to check on the progress.

To open n8n at any time, visit <https://subdomain.example.com/> (your domain) in your browser.

With your n8n instance, you’ll have access to over 400 integrations and a
suite of basic and advanced AI nodes such as
[AI Agent](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/),
[Text classifier](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.text-classifier/),
and [Information Extractor](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.information-extractor/)
nodes. To keep everything local, just remember to use the Ollama node for your
language model and Qdrant as your vector store.

> [!NOTE]
> This starter kit is designed to help you get started with self-hosted AI
> workflows. While it’s not fully optimized for production environments, it
> combines robust components that work well together for proof-of-concept
> projects. You can customize it to meet your specific needs

## Upgrading

* ### For Nvidia GPU setups:

```bash
docker compose --profile gpu-nvidia pull
docker compose create && docker compose --profile gpu-nvidia up
```

* ### For Mac / Apple Silicon users

```
docker compose pull
docker compose create && docker compose up
```

* ### For Non-GPU setups:

```bash
docker compose --profile cpu pull
docker compose create && docker compose --profile cpu up
```

## 👓 Recommended reading

n8n is full of useful content for getting started quickly with its AI concepts
and nodes. If you run into an issue, go to [support](#support).

- [AI agents for developers: from theory to practice with n8n](https://blog.n8n.io/ai-agents/)
- [Tutorial: Build an AI workflow in n8n](https://docs.n8n.io/advanced-ai/intro-tutorial/)
- [Langchain Concepts in n8n](https://docs.n8n.io/advanced-ai/langchain/langchain-n8n/)
- [Demonstration of key differences between agents and chains](https://docs.n8n.io/advanced-ai/examples/agent-chain-comparison/)
- [What are vector databases?](https://docs.n8n.io/advanced-ai/examples/understand-vector-databases/)

## 🎥 Video walkthrough

- [Installing and using Local AI for n8n](https://www.youtube.com/watch?v=xz_X2N-hPg0)

## 🛍️ More AI templates

For more AI workflow ideas, visit the [**official n8n AI template
gallery**](https://n8n.io/workflows/?categories=AI). From each workflow,
select the **Use workflow** button to automatically import the workflow into
your local n8n instance.

### Learn AI key concepts

- [AI Agent Chat](https://n8n.io/workflows/1954-ai-agent-chat/)
- [AI chat with any data source (using the n8n workflow too)](https://n8n.io/workflows/2026-ai-chat-with-any-data-source-using-the-n8n-workflow-tool/)
- [Chat with OpenAI Assistant (by adding a memory)](https://n8n.io/workflows/2098-chat-with-openai-assistant-by-adding-a-memory/)
- [Use an open-source LLM (via Hugging Face)](https://n8n.io/workflows/1980-use-an-open-source-llm-via-huggingface/)
- [Chat with PDF docs using AI (quoting sources)](https://n8n.io/workflows/2165-chat-with-pdf-docs-using-ai-quoting-sources/)
- [AI agent that can scrape webpages](https://n8n.io/workflows/2006-ai-agent-that-can-scrape-webpages/)

### Local AI templates

- [Tax Code Assistant](https://n8n.io/workflows/2341-build-a-tax-code-assistant-with-qdrant-mistralai-and-openai/)
- [Breakdown Documents into Study Notes with MistralAI and Qdrant](https://n8n.io/workflows/2339-breakdown-documents-into-study-notes-using-templating-mistralai-and-qdrant/)
- [Financial Documents Assistant using Qdrant and](https://n8n.io/workflows/2335-build-a-financial-documents-assistant-using-qdrant-and-mistralai/) [Mistral.ai](http://mistral.ai/)
- [Recipe Recommendations with Qdrant and Mistral](https://n8n.io/workflows/2333-recipe-recommendations-with-qdrant-and-mistral/)

## Tips & tricks

### Accessing local files

The self-hosted AI starter kit will create a shared folder (by default,
located in the same directory) which is mounted to the n8n container and
allows n8n to access files on disk. This folder within the n8n container is
located at `/data/shared` -- this is the path you’ll need to use in nodes that
interact with the local filesystem.

**Nodes that interact with the local filesystem**

- [Read/Write Files from Disk](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.filesreadwrite/)
- [Local File Trigger](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.localfiletrigger/)
- [Execute Command](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.executecommand/)

## 📜 License

This project is licensed under the Apache License 2.0 - see the
[LICENSE](LICENSE) file for details.

## 💬 Support

Join the conversation in the [n8n Forum](https://community.n8n.io/), where you
can:

- **Share Your Work**: Show off what you’ve built with n8n and inspire others
  in the community.
- **Ask Questions**: Whether you’re just getting started or you’re a seasoned
  pro, the community and our team are ready to support with any challenges.
- **Propose Ideas**: Have an idea for a feature or improvement? Let us know!
  We’re always eager to hear what you’d like to see next.
