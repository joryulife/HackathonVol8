
ENV_DEV = $(shell cat .env.dev)
ROOT_PASS = Password!

.PHONY: up
up:
	docker compose up --build -d
#サービスの構築

.PHONY: down
down:
	docker compose down -v
#コンテナ・ネットワーク・イメージ・ボリュームの停止と削除

.PHONY: ps
ps:
	docker compose ps
#コンテナ一覧

.PHONY: test
test:
	docker compose exec api richgo test ./... -v -coverprofile cover.out -count=1
	docker compose exec api go tool cover -html=cover.out -o cover.html

.PHONY: gen
gen:
	go generate ./...

.PHONY: lint
lint:
	docker-compose exec api golangci-lint run -v

.PHONY: test-lint
test-lint: test lint

.PHONY: log
log:
	docker compose logs -f
#コンテナの出力を表示 デフォルト: docker-compose.yml

.PHONY: api-log
api-log:
	docker compose logs -f api
#コンテナの出力を表示 apiの出力

.PHONY: db-log
db-log:
	docker compose logs -f db
#コンテナの出力を表示 dbの出力

# .PHONY: schema-diff
# schema-diff:
# 	docker-compose -f docker-compose.dev.yml exec api skeema diff dev -p$(ROOT_PASS) --allow-unsafe
#
# .PHONY: schema-migrate
# schema-migrate:
# 	docker-compose -f docker-compose.dev.yml exec api sh ./schema.sh $(ROOT_PASS)

.PHONY: deps
deps:
	cat tools.go | awk -F'"' '/_/ {print $$2}' | xargs -tI {} go install {}
