
# **Section 1** : Llama-index and Vector Storage (Search)

This repository has been created for testing and feasibility checks using vector and language chains, specifically llama-index. These libraries are commonly used when implementing Prompt Engineering and consuming one's own data into LLM.  

## Opensearch/Elasticsearch setup

- docker : Opensearch Docker-compose
- docker-elasticsearch : Not working for ES v8, requiring security plug-in with mandatory
- docker-elk : Elasticsearch Docker-compose, Optimized Docker configurations with solving security plug-in issues.
- es-open-search-set-analyzer.py : Put Language analyzer into Open search
- es-open-search.py : Open search sample index creation 
- es-search-set-analyzer.py : Put Language analyzer into Elastic search
- es-search.py : Usage of Elastic search python client
- files : The Sample file for consuming

## Llama-index

- index.json : Vector data local backup created by llama-index
- index_vector_in_opensearch.json : Vector data stored in Open search (Source: `files\all_h1.pdf`)
- llama-index-azure-elk-create.py: llama-index ElasticsearchVectorClient (Unofficial file to manipulate vector search, Created by me, Not Fully Tested)
- llama-index-lang-chain.py : Lang chain memory and agent usage with llama-index
- llama-index-opensearch-create.py : Vector index creation to Open search
- llama-index-opensearch-query-chatgpt.py : Test module to access Azure Open AI Embedding API.
- llama-index-opensearch-query.py : Vector index query with questions to Open search
- llama-index-opensearch-read.py : llama-index ElasticsearchVectorClient (Unofficial file to manipulate vector search, Created by me, Not Fully Tested)
- env.template : The properties. Change its name to `.env` once your values settings is done.

```properties
OPENAI_API_TYPE=azure
OPENAI_API_BASE=https://????.openai.azure.com/
OPENAI_API_VERSION=2022-12-01
OPENAI_API_KEY=<your value in azure>
OPENAI_DEPLOYMENT_NAME_A=<your value in azure>
OPENAI_DEPLOYMENT_NAME_B=<your value in azure>
OPENAI_DEPLOYMENT_NAME_C=<your value in azure>
OPENAI_DOCUMENT_MODEL_NAME=<your value in azure>
OPENAI_QUERY_MODEL_NAME=<your value in azure>

INDEX_NAME=gpt-index-demo
INDEX_TEXT_FIELD=content
INDEX_EMBEDDING_FIELD=embedding
ELASTIC_SEARCH_ID=elastic
ELASTIC_SEARCH_PASSWORD=elastic
OPEN_SEARCH_ID=admin
OPEN_SEARCH_PASSWORD=admin
```

## Milvus Embedded

- `pip install milvus`
- Docker compose: https://milvus.io/docs/install_offline-docker.md
- Milvus Embedded through python console only works in Linux and Mac OS.
- In Windows, Use this link, https://github.com/matrixji/milvus/releases.

```commandline
# Step 1. Start Milvus

1. Unzip the package
Unzip the package, and you will find a milvus directory, which contains all the files required.

2. Start a MinIO service
Double-click the run_minio.bat file to start a MinIO service with default configurations. Data will be stored in the subdirectory s3data.

3. Start an etcd service
Double-click the run_etcd.bat file to start an etcd service with default configurations.

4. Start Milvus service
Double-click the run_milvus.bat file to start the Milvus service.

# Step 2. Run hello_milvus.py

After starting the Milvus service, you can test by running hello_milvus.py. See Hello Milvus for more information.
```

## Conclusion

- Azure Open AI Embedding API,text-embedding-ada-002, supports 1536 dimensions. Elastic search, Lucene based engine, supports 1024 dimensions as a max. Open search can insert 16,000 dimensions as a vector storage. 

- Lang chain interface of Azure Open AI does not support ChatGPT yet. so that reason, need to use alternatives such as `text-davinci-003`.

> from open ai documents: 
text-embedding-ada-002: 
Smaller embedding size. The new embeddings have only 1536 dimensions, one-eighth the size of davinci-001 embeddings, 
making the new embeddings more cost effective in working with vector databases. 
https://openai.com/blog/new-and-improved-embedding-model

> from open search documents: 
However, one exception to this is that the maximum dimension count for the Lucene engine is 1,024, compared with
16,000 for the other engines. https://opensearch.org/docs/latest/search-plugins/knn/approximate-knn/

> from llama-index examples: 
However, the examples in llama-index use 1536 vector size.


# **Section 2** : azure-search-openai-demo setup steps

The files in this directory, `extra_steps`, have been created for managing extra configurations and steps for launching the demo repository.

https://github.com/Azure-Samples/azure-search-openai-demo

![Screenshot](./files/capture_azure_demo.png "Main")

## extra_steps files

- fix_from_origin : The modified files, setup related
- ms_internal_az_init.ps1 : Powershell script for Azure module installation 
- ms_internal_troubleshootingt.ps1 : Set Specific Subscription Id as default

## Configuration steps

1. (optional) Check Azure module installation in Powershell by running `ms_internal_az_init.ps1` script
2. (optional) Set your Azure subscription Id to default

> Start the following commands in `./azure-search-openai-demo` directory

3. (deploy azure resources) Simply Run `azd up`

The azd stores relevant values in the .env file which is stored at `${project_folder}\.azure\az-search-openai-tg\.env`.

```properties
AZURE_ENV_NAME=<your_value_in_azure>
AZURE_LOCATION=<your_value_in_azure>
AZURE_OPENAI_SERVICE=<your_value_in_azure>
AZURE_PRINCIPAL_ID=<your_value_in_azure>
AZURE_SEARCH_INDEX=<your_value_in_azure>
AZURE_SEARCH_SERVICE=<your_value_in_azure>
AZURE_STORAGE_ACCOUNT=<your_value_in_azure>
AZURE_STORAGE_CONTAINER=<your_value_in_azure>
AZURE_SUBSCRIPTION_ID=<your_value_in_azure>
BACKEND_URI=<your_value_in_azure>
```

4. Move to `app` by `cd app` command
5. (sample data loading) Move to `scripts` then Change into Powershell by `Powershell` command, Run `prepdocs.ps1`

- console output (excerpt)

```commandline
        Uploading blob for page 20 -> role_library-20.pdf
        Uploading blob for page 21 -> role_library-21.pdf
        Uploading blob for page 22 -> role_library-22.pdf
        Uploading blob for page 23 -> role_library-23.pdf
        Uploading blob for page 24 -> role_library-24.pdf
        Uploading blob for page 25 -> role_library-25.pdf
        Uploading blob for page 26 -> role_library-26.pdf
        Uploading blob for page 27 -> role_library-27.pdf
        Uploading blob for page 28 -> role_library-28.pdf
        Uploading blob for page 29 -> role_library-29.pdf
        Uploading blob for page 30 -> role_library-30.pdf
Indexing sections from 'role_library.pdf' into search index 'gptkbindex'
Splitting './data\role_library.pdf' into sections
        Indexed 60 sections, 60 succeeded
```

6. Move to `app` by `cd ..` and `cd app` command
7. (locally running) Run `start.cmd`

- console output (excerpt)

```commandline
Building frontend


> frontend@0.0.0 build \azure-search-openai-demo\app\frontend
> tsc && vite build

vite v4.1.1 building for production...
✓ 1250 modules transformed.
../backend/static/index.html                    0.49 kB
../backend/static/assets/github-fab00c2d.svg    0.96 kB
../backend/static/assets/index-184dcdbd.css     7.33 kB │ gzip:   2.17 kB
../backend/static/assets/index-41d57639.js    625.76 kB │ gzip: 204.86 kB │ map: 5,057.29 kB

Starting backend

 * Serving Flask app 'app'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
127.0.0.1 - - [13/Apr/2023 14:25:31] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [13/Apr/2023 14:25:31] "GET /assets/index-184dcdbd.css HTTP/1.1" 200 -
127.0.0.1 - - [13/Apr/2023 14:25:31] "GET /assets/index-41d57639.js HTTP/1.1" 200 -
127.0.0.1 - - [13/Apr/2023 14:25:31] "GET /assets/github-fab00c2d.svg HTTP/1.1" 200 -
127.0.0.1 - - [13/Apr/2023 14:25:32] "GET /favicon.ico HTTP/1.1" 304 -
127.0.0.1 - - [13/Apr/2023 14:25:42] "POST /chat HTTP/1.1" 200 -
```

Running from second times

1. Move to `app` by `cd ..` and `cd app` command
2. (locally running) Run `start.cmd`

# **Section 3** : Microsoft Semantic Kernel with Azure Cosmos DB

Microsoft Langchain Library supports C# and Python and offers several features, some of which are still in development and may be unclear on how to implement. However, it is simple, stable, and faster than Python-based open-source software. The features listed on the link include: [Semantic Kernel Feature Matrix](https://github.com/microsoft/semantic-kernel/blob/main/FEATURE_MATRIX.md)

![Screenshot](./files/sk-flow.png "SK")

This section includes how to utilize Azure Cosmos DB for vector storage and vector search by leveraging the Semantic-Kernel.

## Semantic-Kernel

- appsettings.template.json : Enviroment value configuration file.
- ComoseDBVectorSearch.cs : Vector Search using Azure Cosmos DB
- CosmosDBKernelBuild.cs : Kernel Build code (test)
- CosmosDBVectorStore.cs : Embedding Text and store it to Azure Cosmos DB
- LoadDocumentPage.cs : PDF spilitter class. Split the text to unit of section
- LoadDocumentPageOutput : LoadDocumentPage class generated output
- MemoryContextAndPlanner.cs : Test code of context and planner
- MemoryConversationHistory.cs : Test code of conversation history
- Program.cs : Run a demo. Program Entry point
- SemanticFunction.cs : Test code of conversation history
- semanticKernelCosmos.csproj : C# Project file
- Settings.cs : Environment value class
- SkillBingSearch.cs : Bing Search Skill
- SkillDALLEImgGen.cs : DALLE Skill (Only OpenAI, Azure Open AI not supports yet)

## Environment values

```json
{
  "Type": "azure",
  "Model": "<model_deployment_name>",
  "EndPoint": "https://<your-endpoint-value>.openai.azure.com/",
  "AOAIApiKey": "<your-key>",
  "OAIApiKey": "",
  "OrdId": "-", //The value needs only when using Open AI.
  "BingSearchAPIKey": "<your-key>",
  "aoaiDomainName": "<your-endpoint-value>",
  "CosmosConnectionString": "<cosmos-connection-string>"
}
```

## References

- [Microsoft Semantic Kernel](https://github.com/microsoft/semantic-kernel)

- [LangChain](https://python.langchain.com/en/latest/index.html)

- [llama-index](https://github.com/jerryjliu/llama_index)

- [ChatGPT + Enterprise data with Azure OpenAI and Cognitive Search](https://github.com/Azure-Samples/azure-search-openai-demo)

- [Can ChatGPT work with your enterprise data?](https://www.youtube.com/watch?v=tW2EA4aZ_YQ)

- [Azure OpenAI と Azure Cognitive Search の組み合わせを考える](https://qiita.com/nohanaga/items/59e07f5e00a4ced1e840)
