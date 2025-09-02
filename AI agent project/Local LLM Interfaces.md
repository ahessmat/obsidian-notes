```python
#!/usr/bin/env python3
"""
Local LLM Interfaces for CTF Framework
Supports Ollama, Transformers, llama-cpp, and other local model backends
"""

import os
import json
import time
import requests
from typing import Tuple, Optional, Dict, Any
from ctf_framework import LLMInterface

# Ollama Interface (Recommended for local models)
class OllamaInterface(LLMInterface):
    """Interface for Ollama local models"""
    
    def __init__(self, model: str = "llama2:7b-chat", base_url: str = "http://localhost:11434"):
        self.model = model
        self.base_url = base_url
        self.api_url = f"{base_url}/api/generate"
        
        # Test connection
        self._test_connection()
    
    def _test_connection(self):
        """Test if Ollama is running and model is available"""
        try:
            response = requests.get(f"{self.base_url}/api/tags", timeout=5)
            if response.status_code == 200:
                models = response.json().get("models", [])
                model_names = [m["name"] for m in models]
                if self.model not in model_names:
                    print(f"⚠️  Model {self.model} not found. Available: {model_names}")
                    print(f"   Run: ollama pull {self.model}")
        except requests.exceptions.RequestException as e:
            print(f"⚠️  Cannot connect to Ollama at {self.base_url}: {e}")
            print("   Make sure Ollama is running: sudo systemctl start ollama")
    
    def generate_response(self, prompt: str, max_tokens: int = 2000) -> Tuple[str, int]:
        """Generate response using Ollama API"""
        try:
            payload = {
                "model": self.model,
                "prompt": prompt,
                "stream": False,
                "options": {
                    "num_predict": max_tokens,
                    "temperature": 0.1,
                    "top_p": 0.9,
                }
            }
            
            response = requests.post(
                self.api_url,
                json=payload,
                timeout=120  # Local models can be slow
            )
            
            if response.status_code == 200:
                result = response.json()
                content = result.get("response", "")
                
                # Estimate tokens (rough approximation)
                tokens = len(prompt.split()) + len(content.split())
                
                return content, tokens
            else:
                return f"Ollama API Error: {response.status_code} - {response.text}", 0
                
        except requests.exceptions.RequestException as e:
            return f"Connection error: {str(e)}", 0
        except Exception as e:
            return f"Error: {str(e)}", 0
    
    def get_model_name(self) -> str:
        return f"ollama-{self.model}"

# Transformers Interface (Direct HuggingFace models)
class TransformersInterface(LLMInterface):
    """Interface for HuggingFace Transformers models"""
    
    def __init__(self, model_name: str = "microsoft/DialoGPT-medium", device: str = "auto"):
        self.model_name = model_name
        self.device = device
        self.tokenizer = None
        self.model = None
        
        self._load_model()
    
    def _load_model(self):
        """Load the model and tokenizer"""
        try:
            from transformers import AutoTokenizer, AutoModelForCausalLM
            import torch
            
            print(f"Loading {self.model_name}...")
            
            self.tokenizer = AutoTokenizer.from_pretrained(self.model_name)
            
            # Add padding token if not present
            if self.tokenizer.pad_token is None:
                self.tokenizer.pad_token = self.tokenizer.eos_token
            
            # Load model with appropriate settings
            if self.device == "auto":
                device = "cuda" if torch.cuda.is_available() else "cpu"
            else:
                device = self.device
            
            self.model = AutoModelForCausalLM.from_pretrained(
                self.model_name,
                torch_dtype=torch.float16 if device == "cuda" else torch.float32,
                device_map=device if device == "cuda" else None,
                low_cpu_mem_usage=True
            )
            
            if device == "cpu":
                self.model = self.model.to(device)
            
            print(f"✅ Model loaded on {device}")
            
        except ImportError as e:
            print(f"❌ Missing dependencies: {e}")
            print("Install with: pip install transformers torch")
        except Exception as e:
            print(f"❌ Failed to load model: {e}")
    
    def generate_response(self, prompt: str, max_tokens: int = 2000) -> Tuple[str, int]:
        """Generate response using Transformers"""
        if not self.model or not self.tokenizer:
            return "Model not loaded", 0
        
        try:
            import torch
            
            # Encode input
            inputs = self.tokenizer.encode(prompt, return_tensors="pt", truncate=True)
            inputs = inputs.to(self.model.device)
            
            # Generate
            with torch.no_grad():
                outputs = self.model.generate(
                    inputs,
                    max_new_tokens=max_tokens,
                    do_sample=True,
                    temperature=0.1,
                    top_p=0.9,
                    pad_token_id=self.tokenizer.pad_token_id,
                    eos_token_id=self.tokenizer.eos_token_id,
                )
            
            # Decode response
            generated_tokens = outputs[0][inputs.shape[1]:]  # Remove input tokens
            response = self.tokenizer.decode(generated_tokens, skip_special_tokens=True)
            
            total_tokens = len(outputs[0])
            
            return response.strip(), total_tokens
            
        except Exception as e:
            return f"Generation error: {str(e)}", 0
    
    def get_model_name(self) -> str:
        return f"transformers-{self.model_name.split('/')[-1]}"

# llama-cpp-python Interface (for GGUF models)
class LlamaCppInterface(LLMInterface):
    """Interface for llama-cpp-python (GGUF format models)"""
    
    def __init__(self, model_path: str, n_ctx: int = 4096, n_gpu_layers: int = 0):
        self.model_path = model_path
        self.n_ctx = n_ctx
        self.n_gpu_layers = n_gpu_layers
        self.llm = None
        
        self._load_model()
    
    def _load_model(self):
        """Load the GGUF model"""
        try:
            from llama_cpp import Llama
            
            if not os.path.exists(self.model_path):
                print(f"❌ Model file not found: {self.model_path}")
                print("   Download GGUF models from HuggingFace or other sources")
                return
            
            print(f"Loading GGUF model: {self.model_path}")
            
            self.llm = Llama(
                model_path=self.model_path,
                n_ctx=self.n_ctx,
                n_gpu_layers=self.n_gpu_layers,
                verbose=False
            )
            
            print("✅ GGUF model loaded")
            
        except ImportError:
            print("❌ llama-cpp-python not installed")
            print("Install with: pip install llama-cpp-python")
        except Exception as e:
            print(f"❌ Failed to load model: {e}")
    
    def generate_response(self, prompt: str, max_tokens: int = 2000) -> Tuple[str, int]:
        """Generate response using llama-cpp"""
        if not self.llm:
            return "Model not loaded", 0
        
        try:
            output = self.llm(
                prompt,
                max_tokens=max_tokens,
                temperature=0.1,
                top_p=0.9,
                echo=False
            )
            
            response = output["choices"][0]["text"]
            tokens_used = output["usage"]["total_tokens"]
            
            return response.strip(), tokens_used
            
        except Exception as e:
            return f"Generation error: {str(e)}", 0
    
    def get_model_name(self) -> str:
        return f"llamacpp-{os.path.basename(self.model_path)}"

# vLLM Interface (High-performance serving)
class vLLMInterface(LLMInterface):
    """Interface for vLLM high-performance serving"""
    
    def __init__(self, model_name: str = "microsoft/DialoGPT-medium", tensor_parallel_size: int = 1):
        self.model_name = model_name
        self.tensor_parallel_size = tensor_parallel_size
        self.llm = None
        
        self._load_model()
    
    def _load_model(self):
        """Load model with vLLM"""
        try:
            from vllm import LLM, SamplingParams
            
            print(f"Loading {self.model_name} with vLLM...")
            
            self.llm = LLM(
                model=self.model_name,
                tensor_parallel_size=self.tensor_parallel_size,
                gpu_memory_utilization=0.8,
                max_num_batched_tokens=4096
            )
            
            self.sampling_params = SamplingParams(
                temperature=0.1,
                top_p=0.9,
                max_tokens=2000
            )
            
            print("✅ vLLM model loaded")
            
        except ImportError:
            print("❌ vLLM not installed")
            print("Install with: pip install vllm")
        except Exception as e:
            print(f"❌ Failed to load model: {e}")
    
    def generate_response(self, prompt: str, max_tokens: int = 2000) -> Tuple[str, int]:
        """Generate response using vLLM"""
        if not self.llm:
            return "Model not loaded", 0
        
        try:
            from vllm import SamplingParams
            
            # Update sampling params for this request
            sampling_params = SamplingParams(
                temperature=0.1,
                top_p=0.9,
                max_tokens=max_tokens
            )
            
            outputs = self.llm.generate([prompt], sampling_params)
            response = outputs[0].outputs[0].text
            
            # Estimate tokens
            tokens = len(prompt.split()) + len(response.split())
            
            return response.strip(), tokens
            
        except Exception as e:
            return f"Generation error: {str(e)}", 0
    
    def get_model_name(self) -> str:
        return f"vllm-{self.model_name.split('/')[-1]}"

# Model Manager for easy switching
class LocalModelManager:
    """Manager for different local model backends"""
    
    def __init__(self):
        self.available_models = {}
        self._discover_models()
    
    def _discover_models(self):
        """Discover available local models"""
        # Check Ollama models
        try:
            response = requests.get("http://localhost:11434/api/tags", timeout=5)
            if response.status_code == 200:
                ollama_models = response.json().get("models", [])
                for model in ollama_models:
                    name = model["name"]
                    self.available_models[f"ollama-{name}"] = {
                        "type": "ollama",
                        "model": name,
                        "interface": OllamaInterface
                    }
        except:
            pass
        
        # Check for GGUF models in common locations
        gguf_paths = [
            "~/models",
            "./models", 
            "/opt/models"
        ]
        
        for path_str in gguf_paths:
            path = os.path.expanduser(path_str)
            if os.path.exists(path):
                for file in os.listdir(path):
                    if file.endswith(('.gguf', '.bin')):
                        model_path = os.path.join(path, file)
                        name = f"gguf-{os.path.splitext(file)[0]}"
                        self.available_models[name] = {
                            "type": "llamacpp",
                            "model_path": model_path,
                            "interface": LlamaCppInterface
                        }
    
    def get_interface(self, model_key: str) -> Optional[LLMInterface]:
        """Get interface for specified model"""
        if model_key not in self.available_models:
            print(f"Model {model_key} not available")
            print(f"Available models: {list(self.available_models.keys())}")
            return None
        
        model_info = self.available_models[model_key]
        interface_class = model_info["interface"]
        
        try:
            if model_info["type"] == "ollama":
                return interface_class(model_info["model"])
            elif model_info["type"] == "llamacpp":
                return interface_class(model_info["model_path"])
            else:
                return interface_class()
        except Exception as e:
            print(f"Failed to create interface for {model_key}: {e}")
            return None
    
    def list_available(self) -> Dict[str, Dict]:
        """List all available models"""
        return self.available_models

# Example usage and testing
def test_local_models():
    """Test local model interfaces"""
    
    print("🧪 Testing Local Model Interfaces")
    print("=" * 40)
    
    manager = LocalModelManager()
    available = manager.list_available()
    
    if not available:
        print("❌ No local models found")
        print("\nTo get started:")
        print("1. Install Ollama: curl -fsSL https://ollama.ai/install.sh | sh")
        print("2. Pull a model: ollama pull llama2:7b-chat")
        print("3. Or download GGUF models to ~/models/")
        return
    
    print(f"Found {len(available)} local models:")
    for name, info in available.items():
        print(f"  - {name} ({info['type']})")
    
    # Test first available model
    test_model = next(iter(available.keys()))
    print(f"\n🧪 Testing {test_model}...")
    
    interface = manager.get_interface(test_model)
    if interface:
        test_prompt = "You are a cybersecurity expert. How would you approach a SQL injection challenge?"
        response, tokens = interface.generate_response(test_prompt, max_tokens=100)
        
        print(f"Response: {response[:200]}...")
        print(f"Tokens: {tokens}")
        print("✅ Local model test successful!")
    else:
        print("❌ Failed to create interface")

if __name__ == "__main__":
    test_local_models()
```