# Introduction

This repository provides a `docker-compose.yml` ready-to-use Docker configuration to simplify the deployment of the official GitLab Community Edition (CE) Docker container. For comprehensive details about extra configurations, please refer to the [GitLab official Docker installation guide](https://docs.gitlab.com/ee/install/docker.html).

## Repository Contents

This repository includes:

- **Docker Compose File**: A `docker-compose.yml` file that configures the GitLab Docker container. Highlights of this setup include:
  - **Persistent Data Storage**: Configures three volumes:
    - `./volumes/gitlab/config` at `/etc/gitlab` for storing GitLab configuration files.
    - `./volumes/gitlab/logs` at `/var/log/gitlab` for storing logs.
    - `./volumes/gitlab/data` at `/var/opt/gitlab` for storing GitLab data.
    
    This setup ensures that your GitLab configurations, logs, and data are preserved when the container is restarted.

## Getting Started

### Important Preliminary Steps

Before starting the quick setup, please note the following adjustments:

1. **Important Note on Port Mapping**: The `docker-compose.yml` file contains a port mapping to `127.0.0.1:10070:80`, limiting access to the localhost only. To make the service accessible from other machines or publicly, change the IP address in the port mapping. For instance, use `0.0.0.0` to bind to all network interfaces, updating the ports line to `- "0.0.0.0:10070:80"` for broader network access.

### Quick Setup

1. **Clone the Repository**:
   Clone this repository to your local machine using the following Git command:
   ```bash
   git clone https://github.com/ramuyk/docker-gitlab-ce.git
   cd docker-gitlab-ce
   ```

2. **Start GitLab**:
   Use the following command to start the GitLab service using Docker Compose:
   ```bash
   docker compose up -d
   ```

3. **Configure the Root Account**:
   After starting the GitLab container, configure the administrative user by entering the container's Rails console:
   ```bash
   docker exec -it gitlab-prod gitlab-rails console
   ```
   Then set the username and password for the `root` user:
   ```ruby
   user = User.find_by(username: 'root')
   user.username = 'username'
   user.password = 'mynewpassword123'
   user.password_confirmation = 'mynewpassword123'
   user.save!
   ```
   Replace `'username'` and `'mynewpassword123'` with your desired administrative username and a secure password.

4. **Access GitLab**:
   Open a web browser and navigate to `http://localhost:10070` to access the GitLab web interface. Log in with the username and password that you configured in step 3. This account is the administrative account with full access to the GitLab server.
