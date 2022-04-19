# CE7455 Final Project - Automatic code and documentation generation

### Team members: Srivatsan Chandrasekar, Ponmithiran Kannan, Nikhil Nallapati

### Downloading the datasets:
**Concode dataset**


* [Download Link](https://github.com/microsoft/CodeXGLUE/tree/main/Text-Code/text-to-code/dataset/concode)
* Download the 3 json files to nl2code/data/concode

**CodeSearchNet dataset**

* [Download Link](https://drive.google.com/open?id=1rd2Tc6oUWBo7JouwexW3ksQ0PaOhUr6h)
* Download and unzip into code2nl/data/code2nl


### Code generation (English to Java) using concode dataset
**Fine-tuning base PLBART for code generation** (navigate to nl2code directory)
```commandline
python run_nl2code.py --do_train --do_eval --model_type=plbart --model_name_or_path=uclanlp/plbart-base --train_filename=data/concode/train.json --dev_filename=data/concode/dev.json --output_dir=model/java/plbart --max_source_length=256 --max_target_length=256 --beam_size=10 --train_batch_size=1 --eval_batch_size=1 --learning_rate=5e-5 --train_steps=50000 --eval_steps=5000
```

**Running only eval using locally saved model**
```commandline
python run_nl2code.py --do_test --model_type=plbart --model_name_or_path=<path to locally saved plbart model> --dev_filename=data/concode/dev.json --output_dir=model/java/plbart --max_source_length=256 --max_target_length=256 --beam_size=10 --eval_batch_size=1
```

**Fine-tuning base CodeBERT for code generation** (navigate to nl2code directory)
```commandline
python run_nl2code.py --do_train --do_eval --model_type=roberta --model_name_or_path=microsoft/codebert-base --train_filename=data/concode/train.json --dev_filename=data/concode/dev.json --output_dir=model/java/codebert --max_source_length=256 --max_target_length=256 --beam_size=10 --train_batch_size=1 --eval_batch_size=1 --learning_rate=5e-5 --train_steps=50000 --eval_steps=5000
```


**NOTE:**
* Change train and eval batch size according to your GPU memory. 
* Since the decoder performs greedy decoding, eval takes a long time to run. So you may want to make eval less frequent (increase eval_steps)
* **Generated output (dev.output) and ground truth (dev.gold) text files can be found in the <output_dir>**
* CodeBert needs further fine-tuning for the code-generation task.

### Code documentation generation/summarization (Python to English) using CodeSearchNet dataset
**Fine-tuning base PLBART for documentation generation** (navigate to code2nl directory)
```commandline
python run_code2nl.py --do_train --do_eval --model_type=plbart --model_name_or_path=uclanlp/plbart-base --train_filename=data/code2nl/CodeSearchNet/python/train.jsonl --dev_filename=data/code2nl/CodeSearchNet/python/valid.jsonl --output_dir=model/python/plbart --max_source_length=256 --max_target_length=128 --beam_size=10 --train_batch_size=1 --eval_batch_size=1 --learning_rate=5e-5 --train_steps=50000 --eval_steps=5000
```

**Fine-tuning base CodeBERT for documentation generation** (navigate to code2nl directory)
```commandline
python run_code2nl.py --do_train --do_eval --model_type=roberta --model_name_or_path=microsoft/codebert-base --train_filename=data/code2nl/CodeSearchNet/python/train.jsonl --dev_filename=data/code2nl/CodeSearchNet/python/valid.jsonl --output_dir=model/python/codebert --max_source_length=256 --max_target_length=128 --beam_size=10 --train_batch_size=1 --eval_batch_size=1 --learning_rate=5e-5 --train_steps=50000 --eval_steps=5000
```

**NOTE:**
* Change train and eval batch size according to your GPU memory. 
* Since the decoder performs greedy decoding, eval takes a long time to run. So you may want to make eval less frequent (increase eval_steps)
* **Generated output (dev.output) and ground truth (dev.gold) text files can be found in the <output_dir>**