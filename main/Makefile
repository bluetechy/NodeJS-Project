########################################################################
# CONSTANTS FOR MAKEFILE
########################################################################

PROJECT_NAME = project

########################################################################
# RULES FOR DOCKER COMPOSE (dev)
########################################################################

# make dev-up
dev-up: dev-down
	docker-compose -p $(PROJECT_NAME) -f docker-compose-dev.yml up -d

# make dev-build
# make dev-build CONTAINER=project-app
dev-build: dev-down
ifndef CONTAINER
	docker-compose -p $(PROJECT_NAME) -f docker-compose-dev.yml build
else
	docker-compose -p $(PROJECT_NAME) -f docker-compose-dev.yml build $(CONTAINER)
endif

# make dev-down
dev-down:
	docker-compose -p $(PROJECT_NAME) -f docker-compose-dev.yml down
	docker-compose -p $(PROJECT_NAME) -f docker-compose-dev.yml rm -f

########################################################################
# RULES FOR DOCKER COMPOSE (prod)
########################################################################

# make prod-up
prod-up: prod-down
	docker-compose -p $(PROJECT_NAME) -f docker-compose-prod.yml up -d

# make prod-build
# make prod-build CONTAINER=project-app
prod-build: prod-down
ifndef CONTAINER
	docker-compose -p $(PROJECT_NAME) -f docker-compose-prod.yml build
else
	docker-compose -p $(PROJECT_NAME) -f docker-compose-prod.yml build $(CONTAINER)
endif

# make prod-down
prod-down:
	docker-compose -p $(PROJECT_NAME) -f docker-compose-prod.yml down
	docker-compose -p $(PROJECT_NAME) -f docker-compose-prod.yml rm -f
