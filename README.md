# Mailcatcher Helm Chart
This Helm chart installs the Mailcatcher service into a Kubernetes cluster. Mailcatcher is a simple SMTP server and web interface designed for testing email-sending applications by capturing and displaying emails locally instead of sending them to their intended recipients.

## Application Source:

https://mailcatcher.me/

https://github.com/sj26/mailcatcher
## Features
Deploys the Mailcatcher service to your Kubernetes cluster.
Configurable SMTP and HTTP ports.
Lightweight and easy to set up.
Ideal for testing and development environments.
## Prerequisites
Helm 3.0+

A running Kubernetes cluster with sufficient resources.
## Installation

### Install the Chart
```
helm install mailcatcher . --namespace <your-namespace>
Replace <your-namespace> with the namespace where you want to deploy Mailcatcher.
```

### Configuration
The following table lists the configurable parameters of the Mailcatcher chart and their default values:

| Parameter                 | 	Description                             | 	Default                   |
|---------------------------|------------------------------------------|----------------------------|
| replicaCount              | 	Number of replicas for the deployment   | 	1                         |
| image.repository          | 	github image repository for Mailcatcher | 	gchr.io/nrcdt/mailcatcher |
| image.tag                 | 	Tag of the Mailcatcher Docker image     | 	latest                    |
| image.pullPolicy          | 	Image pull policy                       | 	IfNotPresent              |
| smtp_service.port         | 	Port for the SMTP server                | 	25                        |
| smtp_service.type         | 	Kubernetes service type                 | 	ClusterIP                 |
| http_service.port         | 	Port for the web interface              | 	80                        |
| http_service.type         | 	Kubernetes service type                 | 	ClusterIP                 |
| ingress.enabled           | 	Enable Ingress resource                 | 	false                     |
| ingress.hosts             | 	List of ingress hosts	                  | []                         |
| ingress.htpasswd.enabled  | Enable htpasswd                          | false                      |
| ingress.htpasswd.user     | Username for webinterface                |                            |
| ingress.htpasswd.password | Password for webinterface                |                            |
| resources                 | 	Resource requests and limits	           | {}                         |
| nodeSelector              | 	Node selector for scheduling            | 	{}                        |
| tolerations               | 	Tolerations for scheduling	             | []                         |
| affinity                  | 	Pod affinity rules                      | 	{}                        |


### Accessing the Service
#### SMTP Server
Send emails to the SMTP server at service-name.namespace.svc.cluster.local:smtp-port.

#### Web Interface
Access the web interface to view captured emails:

If using a ClusterIP service:

Use kubectl port-forward to access the service locally:
```
kubectl port-forward service/mailcatcher 1080:1080 -n <your-namespace>
```
Open your browser and navigate to http://localhost:1080.
If using an Ingress resource:

Navigate to the specified hostname (e.g., http://mailcatcher.example.com).
Uninstallation
To uninstall the Mailcatcher release:
```
helm uninstall mailcatcher --namespace <your-namespace>
```

### Troubleshooting
No emails are captured: Ensure your application is configured to send emails to the SMTP server at the correct address and port.

Cannot access the web interface: Verify the service type (ClusterIP, NodePort, or Ingress) and any network policies or firewalls in place.
