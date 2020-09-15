# ローカル環境でのセットアップ

## docker
```
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.2.0

docker run -d \
  --name dev-es \ 
  -p 9200:9200 -p 9300:9300 \
  -e "discovery.type=single-node" \
  docker.elastic.co/elasticsearch/elasticsearch:7.2.0
```