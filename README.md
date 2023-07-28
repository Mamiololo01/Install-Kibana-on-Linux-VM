# Installing-Kibana


Install Kibana with Docker

Docker images for Kibana are available from the Elastic Docker registry. The base image is ubuntu:20.04.

A list of all published Docker images and tags is available at www.docker.elastic.co. The source code is in GitHub.

These images contain both free and subscription features. Start a 30-day trial to try out all of the features.

Run Kibana on Docker for developmentedit

Start an Elasticsearch container for development or testing:

Create a new Docker network for Elasticsearch and Kibana:

docker network create elastic

Pull the Elasticsearch Docker image:

docker pull docker.elastic.co/elasticsearch/elasticsearch:8.9.0

Optional: Verify the Elasticsearch Docker image signature::

wget https://artifacts.elastic.co/cosign.pub

cosign verify --key cosign.pub docker.elastic.co/kibana/kibana:8.9.0

For details about this step, refer to Verify the Elasticsearch Docker image signature in the Elasticsearch documentation.

Start Elasticsearch in Docker:

docker run --name es-node01 --net elastic -p 9200:9200 -p 9300:9300 -t docker.elastic.co/elasticsearch/elasticsearch:8.9.0

When you start Elasticsearch for the first time, the following security configuration occurs automatically:

Certificates and keys are generated for the transport and HTTP layers.

The Transport Layer Security (TLS) configuration settings are written to elasticsearch.yml.

A password is generated for the elastic user.

An enrollment token is generated for Kibana.

You might need to scroll back a bit in the terminal to view the password and enrollment token.

Copy the generated password and enrollment token and save them in a secure location. These values are shown only when you start Elasticsearch for the first time. You’ll use these to enroll Kibana with your Elasticsearch cluster and log in.

In a new terminal session, start Kibana and connect it to your Elasticsearch container:

docker pull docker.elastic.co/kibana/kibana:8.9.0

docker run --name kib-01 --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.9.0

Pull the Kibana Docker image:

docker pull docker.elastic.co/kibana/kibana:8.9.0

Optional: Verify the Kibana Docker image signature::

wget https://artifacts.elastic.co/cosign.pub

cosign verify --key cosign.pub docker.elastic.co/kibana/kibana:8.9.0

For details about this step, refer to Verify the Elasticsearch Docker image signature in the Elasticsearch documentation.

Start Kibana in Docker:

docker run --name kib-01 --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.9.0

When you start Kibana, a unique link is output to your terminal.

To access Kibana, click the generated link in your terminal.

In your browser, paste the enrollment token that you copied when starting Elasticsearch and click the button to connect your Kibana instance with Elasticsearch.

Log in to Kibana as the elastic user with the password that was generated when you started Elasticsearch.

Generate passwords and enrollment tokensedit


<img width="1271" alt="Screenshot 2023-07-26 at 23 13 45" src="https://github.com/Mamiololo01/Install-Kibana-on-Linux-VM/assets/67044030/b43a85db-65cf-4d2f-bce3-03381f794b10">


<img width="1011" alt="Screenshot 2023-07-27 at 00 21 24" src="https://github.com/Mamiololo01/Install-Kibana-on-Linux-VM/assets/67044030/4d35cbf9-8595-4344-a3f4-220554bdc79e">

If you need to reset the password for the elastic user or other built-in users, run the elasticsearch-reset-password tool. This tool is available in the Elasticsearch bin directory of the Docker container.

For example, to reset the password for the elastic user:

docker exec -it es-node01 /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic

If you need to generate new enrollment tokens for Kibana or Elasticsearch nodes, run the elasticsearch-create-enrollment-token tool. This tool is available in the Elasticsearch bin directory of the Docker container.

For example, to generate a new enrollment token for Kibana:

docker exec -it es-node01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana

Remove Docker containersedit

To remove the containers and their network, run:

docker network rm elastic

docker rm es-node01

docker rm kib-01
