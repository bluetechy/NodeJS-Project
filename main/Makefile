########################################################################
# RULES FOR DOCKER COMPOSE
########################################################################

up: down
	docker-compose -p project -f docker-compose.yml up -d
down:
	docker-compose -p project -f docker-compose.yml down
	docker-compose -p project -f docker-compose.yml rm -f
