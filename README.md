# Grammar Correction & Paraphrasing System

## Project Structure

```
grammar-paraphrase-system/
├── README.md
├── requirements.txt
├── .env.template
├── .gitignore
├── docker-compose.yml
├── Dockerfile
│
├── src/
│   ├── __init__.py
│   │
│   ├── domain/
│   │   ├── __init__.py
│   │   ├── entities/
│   │   │   ├── __init__.py
│   │   │   ├── text_request.py
│   │   │   └── text_response.py
│   │   ├── services/
│   │   │   ├── __init__.py
│   │   │   ├── grammar_service.py
│   │   │   └── paraphrase_service.py
│   │   └── interfaces/
│   │       ├── __init__.py
│   │       ├── model_interface.py
│   │       └── text_processor_interface.py
│   │
│   ├── infrastructure/
│   │   ├── __init__.py
│   │   ├── models/
│   │   │   ├── __init__.py
│   │   │   ├── model_loader.py
│   │   │   ├── flan_t5_model.py
│   │   │   └── model_config.py
│   │   ├── cache/
│   │   │   ├── __init__.py
│   │   │   └── redis_cache.py
│   │   └── monitoring/
│   │       ├── __init__.py
│   │       └── performance_monitor.py
│   │
│   ├── application/
│   │   ├── __init__.py
│   │   ├── use_cases/
│   │   │   ├── __init__.py
│   │   │   ├── correct_grammar_use_case.py
│   │   │   └── paraphrase_text_use_case.py
│   │   └── dto/
│   │       ├── __init__.py
│   │       ├── grammar_request_dto.py
│   │       └── paraphrase_request_dto.py
│   │
│   ├── presentation/
│   │   ├── __init__.py
│   │   ├── api/
│   │   │   ├── __init__.py
│   │   │   ├── main.py
│   │   │   ├── routers/
│   │   │   │   ├── __init__.py
│   │   │   │   ├── grammar_router.py
│   │   │   │   └── paraphrase_router.py
│   │   │   └── middleware/
│   │   │       ├── __init__.py
│   │   │       └── rate_limiting.py
│   │   └── cli/
│   │       ├── __init__.py
│   │       └── demo_cli.py
│   │
│   └── config/
│       ├── __init__.py
│       ├── settings.py
│       └── prompts.py
│
├── tests/
│   ├── __init__.py
│   ├── unit/
│   ├── integration/
│   └── performance/
│
├── scripts/
│   ├── setup_environment.py
│   ├── download_model.py
│   ├── run_demo.py
│   └── benchmark_model.py
│
├── data/
│   ├── sample_texts/
│   ├── fine_tuning/ (for future fine-tuning data)
│   └── evaluation/
│
├── models/ (downloaded models will be stored here)
│
└── logs/
```

## Step-by-Step Implementation Plan

### Phase 1: Foundation Setup (Day 1)
1. **Environment Setup**
   ```bash
   # Create project
   mkdir grammar-paraphrase-system
   cd grammar-paraphrase-system
   
   # Setup Python environment
   python -m venv venv
   source venv/bin/activate  # or venv\Scripts\activate on Windows
   ```

2. **Install Dependencies**
   ```bash
   pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
   pip install transformers accelerate sentencepiece protobuf
   pip install fastapi uvicorn pydantic
   pip install python-dotenv loguru
   ```

3. **Create Core Domain Entities**
   - Text processing contracts
   - Request/Response models
   - Service interfaces

### Phase 2: Model Integration (Day 2)
1. **Model Infrastructure**
   - Download and configure FLAN-T5
   - GPU memory management
   - Model loading optimization

2. **Domain Services**
   - Grammar correction service
   - Paraphrasing service
   - Prompt engineering

### Phase 3: Application Layer (Day 3)
1. **Use Cases Implementation**
   - Grammar correction workflow
   - Paraphrasing workflow
   - Error handling and validation

2. **CLI Demo**
   - Interactive command-line interface
   - Batch processing capabilities

### Phase 4: API Layer (Day 4)
1. **REST API**
   - FastAPI implementation
   - Request validation
   - Response formatting

2. **Performance Optimization**
   - Caching layer
   - Batch processing
   - Memory management

### Phase 5: Production Ready (Day 5)
1. **Containerization**
   - Docker setup
   - Environment configuration

2. **Monitoring & Logging**
   - Performance metrics
   - Error tracking

## System Prompts for Grammar Correction & Paraphrasing

### Grammar Correction Prompt
```
You are a professional grammar correction assistant. Your task is to correct grammatical errors, spelling mistakes, and punctuation issues while preserving the original meaning and style.

Instructions:
- Fix grammatical errors (subject-verb agreement, tense consistency, etc.)
- Correct spelling mistakes
- Fix punctuation and capitalization
- Maintain the original tone and style
- Keep the meaning exactly the same
- If the text is already correct, return it unchanged

Input: {input_text}
Corrected text:
```

### Paraphrasing Prompt
```
You are a professional paraphrasing assistant. Your task is to rewrite the given text while maintaining the same meaning but using different words and sentence structures.

Instructions:
- Preserve the exact meaning and key information
- Use different vocabulary and sentence structures
- Maintain appropriate tone and formality level
- Ensure the paraphrased text is clear and natural
- Keep similar length unless specified otherwise

Input: {input_text}
Paraphrased text:
```

## Key Configuration Files

### requirements.txt
```
torch>=2.0.0
transformers>=4.30.0
accelerate>=0.20.0
sentencepiece>=0.1.99
protobuf>=3.20.0
fastapi>=0.100.0
uvicorn>=0.22.0
pydantic>=2.0.0
python-dotenv>=1.0.0
loguru>=0.7.0
redis>=4.5.0
numpy>=1.24.0
```

### .env.template
```
# Model Configuration
MODEL_NAME=google/flan-t5-large
MODEL_CACHE_DIR=./models
MAX_LENGTH=512
BATCH_SIZE=4

# API Configuration
API_HOST=0.0.0.0
API_PORT=8000
DEBUG=True

# Hardware Configuration
DEVICE=cuda
GPU_MEMORY_FRACTION=0.8

# Cache Configuration
REDIS_URL=redis://localhost:6379
CACHE_TTL=3600
```

## Getting Started Commands

1. **Setup Project Structure**
   ```bash
   python scripts/setup_environment.py
   ```

2. **Download Model**
   ```bash
   python scripts/download_model.py
   ```

3. **Run Demo**
   ```bash
   python scripts/run_demo.py
   ```

4. **Start API Server**
   ```bash
   uvicorn src.presentation.api.main:app --reload --host 0.0.0.0 --port 8000
   ```

## Next Steps for Fine-tuning (Future)
1. Prepare training dataset with input-output pairs
2. Use Hugging Face's Trainer for fine-tuning
3. Implement LoRA (Low-Rank Adaptation) for efficient fine-tuning
4. Create evaluation metrics and validation pipeline

This architecture follows SOLID principles with clear separation of concerns, dependency injection, and is easily testable and maintainable.
