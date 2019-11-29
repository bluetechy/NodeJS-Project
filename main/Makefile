########################################################################
# CONSTANTS FOR MAKEFILE
########################################################################

PROJECT_UID = project
PROJECT_ENV = dev
PROJECT_NAME = ${PROJECT_UID}-${PROJECT_ENV}

########################################################################
# RULES FOR DOCKER COMPOSE
########################################################################

# make dc-graph-build
# make dc-graph-build SERVICE_NAME=project-app
dc-graph-build: dc-graph-down
ifndef SERVICE_NAME
	docker-compose -p $(PROJECT_NAME) -f docker-compose-${PROJECT_ENV}.yml build
else
	docker-compose -p $(PROJECT_NAME) -f docker-compose-${PROJECT_ENV}.yml build $(SERVICE_NAME)
endif

# make dc-graph-up
dc-graph-up: dc-graph-down
	docker-compose -p ${PROJECT_NAME} -f docker-compose-${PROJECT_ENV}.yml up

# make dc-graph-down
dc-graph-down:
	docker-compose -p ${PROJECT_NAME} -f docker-compose-${PROJECT_ENV}.yml down
	docker-compose -p ${PROJECT_NAME} -f docker-compose-${PROJECT_ENV}.yml rm -f
