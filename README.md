## Ollama + OpenClaw Local LLM Inference

### Upgrade ollama and openclaw in manual

```sh
brew upgrade ollama
openclaw update OR npm install -g openclaw@latest
```

### Recycle the memory in MacOS
```sh
sudo purge
```

```sh
### set the limitation of united memory to 30GBi (30 * 1024 = 30720)
### will be default once the mac restart
sudo sysctl iogpu.wired_limit_mb=30720

# Ensure the LLM remains resident in memory
export OLLAMA_KEEP_ALIVE=-1
export OLLAMA_NUM_CTX=24576
export OLLAMA_FLASH_ATTENTION=1
export OLLAMA_KV_CACHE_TYPE=q8_0
export OLLAMA_NUM_PARALLEL=2
ollama pull qwen3:8b
```

To start ollama now and restart at login:

```sh
  brew services start ollama
```

Or, if you don't want/need a background service you can just run:

```sh
  OLLAMA_FLASH_ATTENTION="1" OLLAMA_KV_CACHE_TYPE="q8_0" /opt/homebrew/opt/ollama/bin/ollama serve
```

### Create custom model from Modelfile

```sh
ollama create zhl-assistant -f ./qwen3_Modelfile
```

### Run the custom model directly with ollama

```sh
ollama run zhl-assistant
```

### List custom models alongside base models

```sh
ollama list
```

### Install OpenClaw

### Launch openclaw with custom model

```sh
# Ensure the LLM remains resident in memory
export OLLAMA_KEEP_ALIVE=-1
ollama launch openclaw --model zhl-assistant
```

> Note: the model list for ollama is configured in `~/.ollama/config.json`

### Restart openclaw gateway

```sh

# set timeout for openclaw
openclaw config set agents.defaults.timeoutSeconds 600
openclaw config set agents.defaults.llm.idleTimeoutSeconds 0

openclaw gateway restart
```

### Make pairing

```sh
openclaw pairing approve feishu <CODE>
```

### Reset the gateway token of openclaw

```sh
openclaw config set gateway.auth.token <YOUR-TOKEN>
openclaw gateway restart
```

## Reference

### How to Set Up Ollama for Local LLM Inference
https://oneuptime.com/blog/post/2026-01-27-ollama-local-llm-inference/view
### Integrate OpenClaw with Ollama
https://docs.ollama.com/integrations/openclaw
### How to Set Up the OpenClaw to use local Ollama LLM
https://docs.openclaw.ai/providers/ollama
### Run OpenClaw locally for free with Ollama and zero API cost
https://lumadock.com/tutorials/openclaw-ollama-local-models-setup

### OpenClaw with Ollama: The Local LLM Guide Nobody Wrote (Context Windows, GPU Reality, and What Actually Works)
https://kaxo.io/insights/openclaw-ollama-local-llm-guide/
