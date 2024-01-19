# Container Discovery Service
Service which is listening on MQTT topics, and on `get` method returns topology of containers from Portainer API. The service is also managing dynamic navigation menu in POC using attached JSON file.

## Dependencies
- Portainer
- MQTT Broker (Mosquitto)
- Portainer access token
- JSON Schema for navigation file
- JSON navigation file

## Config example
- Navigation file example is stored [here](https://gitlab.com/LibertyAces/Product/container-discovery-service/-/blob/dev/conf/navigation_menu_example.json?ref_type=heads)
- Navigation file schema is stored [here](https://gitlab.com/LibertyAces/Product/container-discovery-service/-/blob/dev/conf/navigation_menu_schema_template.json?ref_type=heads)
### Service config
```yaml
portainer-url: http://localhost:9000
mqtt-broker-url: mqtt://localhost:1883
mqtt-containers-pub: /c/running-pipelines/topology
mqtt-containers-sub: /c/running-pipelines/topology/subscribe
mqtt-navigation-topic: /topology
mqtt-navigation-set: /topology/set
navigation-file: /conf/navigation_menu.json
navigation-schema-file: /conf/navigation_schema.json
```
### Docker-compose
```yaml
  container-discovery-service:
    restart: always
    image: <image_url>
    env_file:
      - .discovery_service.env
    volumes:
      - ./discover_service.yaml:/conf/config.yaml:r
      - ./navigation_menu.json:/conf/navigation_menu.json
      - ./navigation_schema.json:/conf/navigation_schema.json
```
### Environment file `.env`
```
PORTAINER_ACCESS_TOKEN=<token-obtained-from-portainer>
```