
SHELL := /bin/bash

service_restart:
	systemctl daemon-reload && systemctl restart traefik

run_compose_up:
	docker compose --env-file ./dc.env -f ./docker-compose.yml up

run_compose_down:
	docker compose --env-file ./dc.env -f ./docker-compose.yml down

run_compose_down_and_delete:
	docker compose --env-file ./dc.env -f ./docker-compose.yml down -v --remove-orphans # --rmi all

check-for-container-updates:
	docker run -it --rm --name watchtower -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --run-once

run_compose_logs:
	docker compose logs --follow

run_web3:
	docker run -it --rm --name web3 --network traefik_default \
		-l traefik.enable=true \
		-l traefik.http.routers.web3.rule='( Host(`{{ ansible_host }}`) && PathPrefix(`/web3`) )' \
		-l traefik.http.middlewares.web3strip.stripprefix.prefixes=/web3 \
		-l traefik.http.routers.web3.middlewares=web3strip \
		-l traefik.http.routers.web3.tls=true nginx:1.23.0

run_web4:
	docker compose -f docker-compose-web4.yml up

generate_basic_auth_password:
	htpasswd -n cs224
