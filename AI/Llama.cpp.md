# Llama.cpp

## Running Qwen3.6 35b MOE model

```
llama-server -m Qwen3.6-35B-A3B-UD-IQ4_XS.gguf -ngl 99 --n-cpu-moe 30 --flash-attn on --cache-type-k q4_0 --cache-type-v q8_0 --ctx-size 32768 -np 1 -t $(nproc) --no-mmap --jinja --reasoning-budget 4096 --reasoning-budget-message "I need to provide my answer now." --reasoning-format deepseek --chat-template-kwargs '{"preserve_thinking":true}' --host 127.0.0.1 --port 3434
```
```
llama-server -m Qwen3.6-35B-A3B-UD-IQ4_XS.gguf -ngl 40 --n-cpu-moe 30 --flash-attn on --cache-type-k q4_0 --cache-type-v q8_0 --ctx-size 32768 -np 1 -t $(nproc) --no-mmap --jinja --reasoning-budget 4096 --reasoning-budget-message "I need to provide my answer now." --reasoning-format deepseek --chat-template-kwargs '{"preserve_thinking":true}' --host 127.0.0.1 --port 3434
```

MOE model with high params runs in 8gb VRAM because only 3 out or 35b parameters are active when running a query.

With dense models, all params are active so 8gb VRAM could not run more than 9b params quantized at q4.

### Requirements

- CUDA Drivers for Nvidia: [CUDA Install in Fedora Immutable systems](https://github.com/ggml-org/llama.cpp/blob/master/docs/backend/CUDA-FEDORA.md)
- CUDA Build of Llama.cpp for Linux (Should be built from source, no ready binary): [Install Instructions](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md#cuda)
- Toolbox distro (as per CUDA Build instructions)  
  Since CUDA cant be installed in immutable systems like bazzite based on fedora.
  ```
  toolbox enter --container fedora-toolbox-41-cuda
  ```

Reading reference: 