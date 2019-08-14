traefik:
	docker service create --name traefik --constraint node.hostname==manager37 --publish 80:80 --publish 8080:8080 --mount type=bind,source=/var/run/docker.sock,target=/var/run/docker.sock --network hbase traefik --docker --docker.swarmMode --docker.domain=cluster.dfstw.com --docker.watch --web --api --logLevel=DEBUG

network:
	docker network create -d overlay --attachable hbase

zookeeper:
	docker stack deploy -c docker-compose-zookeeper.yml zookeeper --with-registry-auth

zookeeper-get-stats:
	docker run -it --rm --network hbase zookeeper:3.4 bash
	# echo "stat" | nc zoo1 2181
	# echo "stat" | nc zoo2 2181
	# echo "stat" | nc zoo3 2181

hadoop:
	docker stack deploy -c docker-compose-hadoop-v3.yml hadoop --with-registry-auth

hbase:
	docker stack deploy -c docker-compose-hbase.yml hbase --with-registry-auth

hbase-clean:
	docker exec -it namenode hadoop fs -rm -r /hbase/*
	docker exec -it zoo bash # bin/zkCli.sh && echo "rmr /hbase"
