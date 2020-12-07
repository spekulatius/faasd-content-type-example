```bash
mkdir php7-json
cd php7-json
faas-cli new php7-json --lang php7 --prefix spekulatius
sd
faas-cli up
faas-cli up -f ./php7-json
faas-cli up -f ./php7-json.yml
faas-cli up -f ./php7-json.yml --gateway faas1:8080
curl -i http://127.0.0.1:8080/function/php7-json.openfaas-fn
curl -i http://faas1:8080/function/php7-json.openfaas-fn
curl -i http://faas1:8080/function/php7-json
curl -i http://faas1:8080/function/php7-json -d 'test'
faas-cli up -f ./php7-json.yml --gateway faas1:8080
curl -i http://faas1:8080/function/php7-json -d 'test'
```
