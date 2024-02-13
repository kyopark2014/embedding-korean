# 한국어 Embedding

## KoSimCSE-robera

Hugging Face의 [KoSimCSE-roberta](https://huggingface.co/BM-K/KoSimCSE-roberta)을 이용한 한글 Embedding입니다. 이것의 [Github repo](https://github.com/BM-K/Sentence-Embedding-is-all-you-need)에 따르면 Base Model은 [KLUE-BERT](https://github.com/KLUE-benchmark/KLUE/blob/main/README.md)입니다.




[KoSimCSE-robera를 이용하여 Endpoint 생성]https://github.com/kyopark2014/embedding-korean/blob/main/KoSimCSE-roberta/embedding-kosimcse.ipynb)에 따라 Endpoint를 생성합니다. 생성한 Embedding은 API를 이용해 호출합니다. 활용 예는 아래와 같습니다. endpoint_name는 노트북을 실행하여 생성된 SageMaker Endpoint입니다.

```python
import boto3
import json

endpoint_name = 'KoSimCSE-roberta-2024-02-13-00-16-45'

sentence = "분당 이마트점에 KT 대리점이 있나요?"
payload = {
    "inputs" : sentence
}

query_response = query_endpoint_embedding_with_json_payload(
    json.dumps(payload).encode("utf-8"), endpoint_name=endpoint_name
)

def query_endpoint_embedding_with_json_payload(encoded_json, endpoint_name, content_type="application/json"):
    client = boto3.client("runtime.sagemaker")
    response = client.invoke_endpoint(
        EndpointName=endpoint_name, ContentType=content_type, Body=encoded_json
    )
    return response
```

Query에 대한 결과는 아래와 같습니다.

```java
{'ResponseMetadata': {'RequestId': '52a457cc-3eb1-4fd7-a2d2-3e9c0149582a',
  'HTTPStatusCode': 200,
  'HTTPHeaders': {'x-amzn-requestid': '52a457cc-3eb1-4fd7-a2d2-3e9c0149582a',
   'x-amzn-invoked-production-variant': 'AllTraffic',
   'date': 'Tue, 13 Feb 2024 00:22:36 GMT',
   'content-type': 'application/json',
   'content-length': '199402',
   'connection': 'keep-alive'},
  'RetryAttempts': 0},
 'ContentType': 'application/json',
 'InvokedProductionVariant': 'AllTraffic',
 'Body': <botocore.response.StreamingBody at 0x7f79d19bf010>}
```




