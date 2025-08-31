```
git clone https://github.com/LG-AI-EXAONE/KoMT-Bench
cd FastChat
bash setting.sh
export OPENAI_API_KEY=<openai key>
export PYTHONPATH=${PYTHONPATH}:${PWD}

cd fastchat/llm_judge/

# Generating model answers
CUDA_VISIBLE_DEVICES=0 python gen_model_answer.py \
		--model-path Qwen/Qwen3-0.6B \
		--model-id Qwen3-0.6B \
		--dtype bfloat16 


# Assessing the model answer through LLM-as-a-judge (here, "gpt-4-0613")
python gen_judgment.py \
    --model-list Qwen3-0.6B


# Giving a penalty to the score of non-Korean responses
cd data/mt_bench/model_judgment
python detector.py \
    --model_id Qwen3-0.6B \
	--judge-model gpt-4-0613


# Showing the evaluation results
cd ../../..
python show_result.py \
    --mode single \
    --input-file ./data/mt_bench/model_judgment/Qwen3-0.6B_single_final.jsonl
```