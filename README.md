# ai_shopper_docker_compose
This docker-compose directory is for tracking/version control purposes only.

To use the docker-compose configuration:
1. Copy or move the `docker-compose.yml` file to the root directory that contains all projects
2. Run docker-compose commands from that root directory

The docker-compose file needs to be at the root level to properly resolve the relative paths and contexts for building the services.

Code bases that you will need:
- [auth] (https://github.com/kdjuwidja/auth)
- [core] (https://github.com/kdjuwidja/ai_shopper_core)
- [service] (https://github.com/kdjuwidja/ai_shopper_service)
- [webapp] (https://github.com/kdjuwidja/ai_shopper_webapp)

## Setup Procedures
1. Clone all required codebases:
   ```bash
   git clone https://github.com/kdjuwidja/auth.git
   git clone https://github.com/kdjuwidja/ai_shopper_core.git
   git clone https://github.com/kdjuwidja/ai_shopper_service.git
   git clone https://github.com/kdjuwidja/ai_shopper_webapp.git
   ```

2. Set up project structure:
   - Move docker-compose files to project root
   - Your folder structure should look like this:
     ```
     root/
     ├── auth/
     ├── core/
     ├── docker-compose/
     ├── service/
     ├── webapp/
     ├── docker-compose-infra.yml
     └── docker-compose.yml
     ```

3. Set up the MySQL database:
   ```bash
   docker-compose -f docker-compose-infra.yml up -d
   ```

4. Configure the database:
   - Log in to MySQL using credentials from `docker-compose-infra.yml`
   - Create schemas:
     ```sql
     CREATE SCHEMA ai_shopper_auth;
     CREATE SCHEMA ai_shopper_core;
     ```
   - Create development user:
     ```sql
     CREATE USER 'ai_shopper_dev'@'%' IDENTIFIED BY 'password';
     GRANT ALL PRIVILEGES ON ai_shopper_auth.* TO 'ai_shopper_dev'@'%';
     GRANT ALL PRIVILEGES ON ai_shopper_core.* TO 'ai_shopper_dev'@'%';
     FLUSH PRIVILEGES;
     ```

5. Configure development environment:
   - Open `docker-compose.yml`
   - Set `IS_LOCAL_DEV=true` in the auth service configuration

6. Start the application:
   ```bash
   docker-compose up
   ```
   This will:
   - Start all services
   - Auto-migrate database tables
   - Create default API clients and users

## Testing
1. Open an incognito browser window
2. Visit http://localhost:3000
