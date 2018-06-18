# NLP-API
1. [Installation](#installation)
2. [API Docs](#api-docs)	
    - [Human Readable](#human-readable)
    - [Machine Readable](#machine-readable)
    - [Editing the Docs](#editing-the-docs)
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

## API Docs

Endpoint documentation is autogenerated with [`drf_yasg`](https://github.com/axnsan12/drf-yasg).  

### Human Readable

Human readable docs are generated using redoc and swagger which can be found at:

- REDOC: [http://nlp-api-staging.api.pal.chat/redoc](http://nlp-api-staging.api.pal.chat/redoc)
- SWAGGER: [http://nlp-api-staging.api.pal.chat/swagger](http://nlp-api-staging.api.pal.chat/swagger)

### Machine Readable

Additionally, the api spec can be downloaded as `json` or `yaml` from the following endpoints:

- JSON: [http://nlp-api-staging.api.pal.chat/swagger.json](http://nlp-api-staging.api.pal.chat/swagger.json)
- YAML: [http://nlp-api-staging.api.pal.chat/swagger.yaml](http://nlp-api-staging.api.pal.chat/swagger.yaml)

### Editing the Docs

Endpoint documentation is generated by the `swagger_auto_schema` decorator on view functions. Brief descriptions of an endpoint's behavior can be provided by editing the function's pydoc. See `/faq/views_v1.py` for some examples. If you want to learn more about the system, there are examples on the `drf_yasg` repo ([https://github.com/axnsan12/drf-yasg](https://github.com/axnsan12/drf-yasg)) and the documentation is quite nice as well ([https://drf-yasg.readthedocs.io/](https://drf-yasg.readthedocs.io/))


## Author's Note
Quite good
