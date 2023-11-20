### Installation Process for Monitoring System

The installation process of the monitoring system is structured into three main components: the installation script, the Docker stack configuration, and the NGINX configuration. This setup ensures a robust, secure, and scalable monitoring system using Grafana, Prometheus, and other tools.

#### Installation Script (`install.sh`):

1. **Docker Environment Setup**:
   - **Docker Check**: The script begins by checking if Docker is installed. If Docker is not found, it provides instructions for installing Docker, which is crucial for running containerized applications.
   - **Docker Swarm Initialization**: It initializes Docker Swarm, a native clustering tool for Docker that turns a group of Docker engines into a single virtual Docker engine.

2. **Software Installation and Updates**:
   - **System Updates**: The script updates the system to ensure all packages are up to date.
   - **Package Installation**: Installs necessary packages including the Docker Compose plugin, firewalld (a dynamic firewall manager), Python 3, and Python packages like `pyyaml` and `requests`.

3. **Script Exporter for Prometheus**:
   - Clones the `script_exporter` repository, which is used for running custom scripts in Prometheus.

4. **SSL Certificate Configuration**:
   - Offers options to set up SSL certificates for secure communication.
   - Users can choose between generating new certificates using Let's Encrypt (requires internet reachability for domain validation) or using existing certificates.
   - Based on the choice, it either runs a certification script or asks for details about the existing certificates.

5. **NGINX Configuration for Grafana**:
   - Updates the NGINX configuration to work with the chosen SSL setup, ensuring secure access to Grafana over HTTPS.

#### Docker Stack (`docker-stack.yml`):

1. **Service Definition**:
   - Configures a set of services including Prometheus, Pushgateway, Script Exporter, Grafana, and NGINX.
   - Each service is defined with specific Docker images and versions.

2. **Port Exposures and Networking**:
   - Exposes the necessary ports for each service: Prometheus on 9090, Pushgateway on 9091, Script Exporter on 9469, and Grafana on 3000.
   - Configures a dedicated network (`monitor-net`) for inter-service communication.

3. **Service Deployment**:
   - Specifies deployment constraints, particularly for node roles within the Docker Swarm, ensuring services are deployed on appropriate nodes.

#### NGINX Configuration:

1. **`nginx:grafana.conf`**:
   - Configures NGINX to listen on port 443 (HTTPS) for secure communication.
   - Includes additional settings from `server_conf` and `proxy_conf`.

2. **`nginx:proxy_conf`**:
   - Sets up NGINX as a reverse proxy to forward requests to Grafana.
   - Preserves important request information like the original host and IP address for accurate logging and security.

3. **`nginx:server_conf`**:
   - Defines the server name for SSL/TLS configuration.
   - Specifies the paths for the SSL certificate and key, aligning with the choices made during the installation script.

#### Grafana Dashboards:

- **Grafana Setup**: Grafana is set up to run on port 3000 and is accessible via the NGINX reverse proxy.
- **Dashboard Creation**: Users can create and manage dashboards within the Grafana UI, using data sourced from Prometheus. This allows for versatile monitoring configurations, suitable for a wide range of applications and environments.

