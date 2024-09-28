# [lol_lore.ipynb](https://github.com/star-bits/mlb-gemma/blob/main/lol_lore.ipynb)

## 한 것
- 롤 챔피언들의 배경 스토리를 스크래핑함. champion_bs.csv에 저장함.
- gpt-4o-mini를 이용해 배경 스토리를 약 1700개의 Q&A pair로 바꿔줌. champion_qa.csv에 저장함. 21분 + 120KRW 소요.
- gemma-2-2b-it를 HF에서 불러와서 QLoRA를 통해 quantize 함.
- 모델에서 linear 레이어(즉, LoRA applicable한 레이어)들을 확인함. 이 target module들을 LoraConfig에 넣어줌.
- (총 레이어 수 422, linear 레이어 수 182, LoRA matrix (A matrix + B matrix) 수 364)
- tokenizer.apply_chat_template()을 이용해 해당 모델의 스페셜 토큰을 훈련 데이터에 적용함.
- 10 epoch을 돌렸고 loss는 0.1638로 줄어듦. 모델의 vocab size는 256,000. (log(256000) = 5.4082)
- Base model과 LoRA를 merge하고 HF에 업로드 하고 다시 HF에서 다운받아서 inference가 제대로 되는지 확인함. 


## 참고 링크

## 더 해 볼 수 있는 것
