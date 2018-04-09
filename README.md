# CC-Project-ELK
Maintain logs using elk service using docker

Step 1: Create 4 linux instances on cloud
Step 2: Select first instance as the elk server instance and install docker in it. Then type the following commands:
Using 2 different Docker images (official Elastic docker images)

- Step 1: Setup Elasticsearch container and verify elastic its working
docker run -d -p 9200:9200 -p 9300:9300 -it -h elasticsearch --name elasticsearch elasticsearch

curl http://localhost:9200/

- Step 2: Setup Kibana container
docker run -d  -p 5601:5601 -h kibana --name kibana --link elasticsearch:elasticsearch kibana

curl http://localhost:9200/_cat/indices

Step 3: On all remaining instances type the following:
- Step 1: apt-get install filebeat
          mv /etc/filebeat/filebeat.yml /etc/filebeat/filebeat.yml.orig
          touch /etc/filebeat/filebeat.yml
          
- Step 2: open the yml file
          nano /etc/filebeat/filebeat.yml
          
- Step 3: Type the following in yml file and then save and close the file
          filebeat.prospectors:
          - input_type: log
           paths:
           - /var/log/apache2/access.log
          output.elasticsearch:
           hosts: ["server-ip-address:9200"]
           
- Step 4: Type:
          service filebeat start
          
- Step 5: Start kibana on server, using https://localhost:5600 on server instance and then change the index on kibana to filebeat-*
          Goto discover and you can see the logs coming from different clients.
