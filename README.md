# [lol_lore.ipynb](https://github.com/star-bits/mlb-gemma/blob/main/lol_lore.ipynb)

## 한 것
- 롤 챔피언들의 배경 스토리를 스크래핑함. champion_bs.csv에 저장함.
- gpt-4o-mini를 이용해 배경 스토리를 약 1700개의 Q&A pair로 바꿔줌. champion_qa.csv에 저장함. 21분 + 120KRW 소요.
- gemma-2-2b-it를 HF에서 불러와서 QLoRA를 통해 quantize 함.
- 모델에서 linear 레이어(즉, LoRA applicable한 레이어)들을 확인함. 이 target module들을 LoraConfig에 넣어줌.
- (총 레이어 수 422, linear 레이어 수 182, LoRA matrix (A matrix + B matrix) 수 364)
- tokenizer.apply_chat_template()을 이용해 해당 모델의 스페셜 토큰을 훈련 데이터에 적용함.
- 10 epoch을 돌렸고 loss는 0.1638로 줄어듦. 모델의 vocab size는 256,000. log(256000) = 5.4082.
- Base model과 LoRA를 merge하고 HF에 업로드 하고 다시 HF에서 다운받아서 inference가 제대로 되는지 확인함. 

## 링크
- HF model: https://huggingface.co/fanlino/gemma-2-2b-lol
- champion_qa.csv: https://huggingface.co/datasets/fanlino/lol-champion-qa
- ollama model: https://huggingface.co/fanlino/gemma-2-2b-lol/blob/main/gemma-2-2B-it-Q4_K_M.bin

## 참고 링크
- full ft
- lora ft
  - https://www.datacamp.com/tutorial/fine-tuning-gemma-2
  - https://www.philschmid.de/fine-tune-google-gemma
  - https://github.com/Taeseo06/SchoolHelperChatbot-Gemma2B-Finetuning/blob/main/Gemma_FineTuning.ipynb
  - 
- run on ollama
  - 모델을 gguf로 변환하고 quantize 해서 https://medium.com/@qdrddr/the-easiest-way-to-convert-a-model-to-gguf-and-quantize-91016e97c987
  - bin 파일과 modelfile을 만들고
  - `ollama create gemma-2-lol:2b -f modelfile`
  - `ollama run gemma-2-lol:2b`
- run on android
  - https://github.com/NSTiwari/Gemma-on-Android
- antropic prompt engineering tutorial
  - https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview
  - https://docs.google.com/spreadsheets/d/19jzLgRruG9kjUQNKtCg1ZjdD6l6weA6qRXG5zLIAhC8
- dpo
  - https://github.com/rasbt/LLMs-from-scratch/blob/main/ch07/04_preference-tuning-with-dpo/dpo-from-scratch.ipynb
- rag
  - lexical + semantic search
  - HyDE Hypothetical Document Embeddings 가상의 아무 답이나 하고 비슷한 걸 찾자
  - parent-child chunking
  - reranker https://aws.amazon.com/ko/blogs/tech/korean-reranker-rag/
