# Elastic Search EI Connector

The Elastic Search [connector](https://docs.wso2.com/display/EI640/Working+with+Connectors) allows you to access the [Elastic Search REST API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html) through WSO2 ESB.
Elastic Search is a distributed, open source search and analytics engine, designed for horizontal
scalability, reliability, and easy management. The Elastic Search REST APIs are exposed using JSON over HTTP.

## Compatibility

| Connector version | Supported Elastic Search version | Supported WSO2 ESB/EI version |
| ------------- | ---------------|------------- |
| [1.0.2](https://github.com/wso2-extensions/esb-connector-elasticsearch/tree/org.wso2.carbon.connector.elasticsearch-1.0.2) | Elastic Search  5.6.3, 5.6.4 |ESB 4.9.0, ESB 5.0.0, EI 6.1.1, EI 6.3.0, EI 6.4.0    |

## Getting started

#### Download and install the connector

1. Download the connector from the [WSO2 Store](https://store.wso2.com/store/assets/esbconnector/details/499bdf23-2f6d-4895-8ee8-02159eb12731) by clicking the Download Connector button.
2. Then you can follow this [Documentation](https://docs.wso2.com/display/EI640/Working+with+Connectors+via+the+Management+Console) to add and enable the connector via the Management Console in your EI instance.
3. For more information on using connectors and their operations in your EI configurations, see [Using a Connector](https://docs.wso2.com/display/EI640/Using+a+Connector).
4. If you want to work with connectors via EI tooling, see [Working with Connectors via Tooling](https://docs.wso2.com/display/EI640/Working+with+Connectors+via+Tooling).

#### Configuring the connector operations

To get started with Elastic Search connector and their operations, see [Configuring Elastic Search Operations](docs/config.md).

## Building From the Source

Follow the steps given below to build the Elastic Search connector from the source code:

1. Get a clone or download the source from [Github](https://github.com/wso2-extensions/esb-connector-elasticsearch).
2. Run the following Maven command from the `esb-connector-elasticsearch` directory: `mvn clean install`.
3. The Elastic Search connector zip file is created in the `esb-connector-elasticsearch/target` directory

## How You Can Contribute

As an open source project, WSO2 extensions welcome contributions from the community.
Check the [issue tracker](https://github.com/wso2-extensions/esb-connector-elasticsearch/issues) for open issues that interest you. We look forward to receiving your contributions.
