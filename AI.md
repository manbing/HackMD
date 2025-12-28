
# Terminology
[Hugging Face](https://huggingface.co/)

OpenAI

LangChain

Voice Agent
Deepgram

# Environment
1
--
因为torchvision安装了gpu版本, 而又不是nvidia卡, 导致的报错. 安装cpu版本的torchvision即可.
首先卸载已有torchaudio torchvision
``` console
pip uninstall torchaudio torchvision
```
然后安装cpu版本, 使用`--extra-index https://download.pytorch.org/whl/cpu/指向index` 即可确定安装哪个版本.
``` console
pip install torchaudio==2.5.1 torchvision==0.20.1 --extra-index https://download.pytorch.org/whl/cpu/
```

2
--

# Component
[AssemblyAI](https://www.assemblyai.com/): Today’s top Voice AI companies rely on AssemblyAI’s speech-to-text (STT) and speech understanding models to launch groundbreaking products fast and scale with ease.

[ElevenLabs](https://elevenlabs.io/): Free AI Voice Generator & Voice Agents Platform. Text-to-Speech (TTS).



# Large Language Model (LLM)
Brain. Thinking

**Example**
Ollama


# Embedding models
nomic-embed-text



# Retrieval Augmented Generation (RAG)

# Livekit

``` console
livekit-server --dev --config ./config.yaml --bind 0.0.0.0
```
[Ingress service](https://docs.livekit.io/transport/self-hosting/ingress/)
[Self-hosting](https://docs.livekit.io/transport/self-hosting/)