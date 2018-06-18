# NLP-API
1. [Installation](#installation)
2. [Endpoints](#endpoints)	
	- [`/faq/v1`](#faqv1)
	- [`/faq`](#faq)
	- [`/admin`](#admin)
	- [`/batch`](#batch) 
3. [Author's Note](#authors-note)

## Installation 

To install, first clone the latest version of the repo from github:
```
git clone git@github:jobpal/nlp-api
```

Next, install spacy and the accompanying language models. Be aware that these models are several gigabytes in size so this step may take some time.
```
pip install spacy==2.0.7
python -m spacy download en_core_web_lg
python -m spacy link en_core_web_lg en --force
pip install https://s3.eu-central-1.amazonaws.com/spacy-int/de_core_fasttext_lg-2.0.0.tar.gz
python -m spacy link de_core_fasttext_lg de --force
```

Finally, install the dependencies listed in `requirements.txt`:
```
pip install -r requirements.txt
```

You can test your build with the following:
```
python -m unittest discover -s . -p '*_test.py'
```

## Endpoints

### `/faq/v1`
- `models`
- `models/{model_id}`
- `models/{model_id}/categories`
- `models/{model_id}/categories/{category_id}`
- `models/{model_id}/contexts`
- `models/{model_id}/answers`
- `models/{model_id}/queries`
- `models/{model_id}/questions`
- `models/{model_id}/questions/{question_id}`
- `models/{model_id}/entities`
- `models/{model_id}/entity_types/{entity_type_id}/entities`
- `models/{model_id}/queries/{query_id}`
- `models/{model_id}/analytics/eval`
- `models/{model_id}/analytics/stats`
- `models/{model_id}/analytics/problems`
- `version`
- `entity_types`
- `entity_types/{entity_type_id}`

### `/batch`

what's this?

## Author's Note
Quite good
