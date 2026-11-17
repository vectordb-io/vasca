# vasca

**[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [Español](README.es.md) | Português do Brasil**

vasca é um projeto de armazenamento de objetos compatível com S3, baseado no MinIO e mantido de forma independente neste repositório.

O código inicial vem da branch `master` do repositório local `minio/`, no commit
[`7aac2a2c5b7c882e68c1ce017d8256be2feea27f`](https://github.com/minio/minio/commit/7aac2a2c5b7c882e68c1ce017d8256be2feea27f).
Foi importado apenas um snapshot do código; este repositório não inclui o histórico de commits do Git do MinIO.
Consulte [UPSTREAM.md](UPSTREAM.md) (em chinês simplificado) para conhecer a origem e o processo de manutenção.

## Compilação e execução

São necessários Git e Go 1.24.8, a versão da toolchain especificada em `go.mod`.

```sh
git clone git@github.com:vectordb-io/vasca.git
cd vasca
go build -tags kqueue -trimpath -o vasca .
./vasca server ./data --address 127.0.0.1:9000 --console-address 127.0.0.1:9001
```

Após a inicialização, a API S3 fica disponível em `http://127.0.0.1:9000` e o console em
`http://127.0.0.1:9001`. As credenciais padrão para desenvolvimento local são `minioadmin:minioadmin`.
Ao implantar, configure suas próprias credenciais por meio de `MINIO_ROOT_USER` e `MINIO_ROOT_PASSWORD`.

## Verificação

```sh
go build -tags kqueue -trimpath -o vasca .
go test -tags kqueue,dev ./internal/...
```

A CI básica executa essas verificações em pushes e pull requests destinados à branch `main`.
Mudanças no comportamento do servidor também devem ser verificadas com os testes de `cmd` correspondentes
e os testes de integração do projeto de origem. Consulte o `Makefile` para conhecer as dependências e os comandos desses testes.

## Escopo da inicialização

Esta inicialização preserva o caminho original do módulo Go `github.com/minio/minio`, os caminhos de importação internos,
as configurações `MINIO_*`, os formatos de dados e o comportamento em tempo de execução.
O executável pode se chamar `vasca`, mas a ajuda e as informações de versão ainda usam o nome MinIO.
Alterações na marca, nos caminhos dos módulos e nos processos de publicação serão feitas separadamente.

A configuração original do GitHub do MinIO está em [docs/upstream/github](docs/upstream/github/)
como referência para manutenção; o vasca usa seu próprio [workflow de CI](.github/workflows/ci.yml).
O `Makefile` e os scripts de publicação originais ainda contêm registros de imagens, URLs de download
e destinos de publicação do MinIO. Configure os destinos deste projeto antes de usar os alvos de publicação.

## Licença e origem

O código continua sob a licença [GNU AGPL v3](LICENSE), preservando os arquivos [NOTICE](NOTICE),
[CREDITS](CREDITS) e os avisos de direitos autorais do projeto de origem nos arquivos-fonte.
vasca é um projeto mantido de forma independente.

Consulte o [README do commit de referência](https://github.com/minio/minio/blob/7aac2a2c5b7c882e68c1ce017d8256be2feea27f/README.md)
para ver a documentação original do projeto de origem.
