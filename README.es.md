# vasca

**[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | Español | [Português do Brasil](README.pt-BR.md)**

vasca es un proyecto de almacenamiento de objetos compatible con S3, basado en MinIO y mantenido de forma independiente en este repositorio.

El código inicial procede de la rama `master` del repositorio local `minio/`, en el commit
[`7aac2a2c5b7c882e68c1ce017d8256be2feea27f`](https://github.com/minio/minio/commit/7aac2a2c5b7c882e68c1ce017d8256be2feea27f).
Solo se importó una instantánea del código; este repositorio no incluye el historial de commits de Git de MinIO.
Consulta [UPSTREAM.md](UPSTREAM.md) (en chino simplificado) para conocer el origen y el proceso de mantenimiento.

## Compilación y ejecución

Se necesitan Git y Go 1.24.8, la versión de la cadena de herramientas especificada en `go.mod`.

```sh
git clone git@github.com:vectordb-io/vasca.git
cd vasca
go build -tags kqueue -trimpath -o vasca .
./vasca server ./data --address 127.0.0.1:9000 --console-address 127.0.0.1:9001
```

Una vez iniciado, la API S3 está disponible en `http://127.0.0.1:9000` y la consola en
`http://127.0.0.1:9001`. Las credenciales predeterminadas para el desarrollo local son `minioadmin:minioadmin`.
Al desplegar, configura tus propias credenciales mediante `MINIO_ROOT_USER` y `MINIO_ROOT_PASSWORD`.

## Verificación

```sh
go build -tags kqueue -trimpath -o vasca .
go test -tags kqueue,dev ./internal/...
```

La CI básica ejecuta estas comprobaciones en los pushes y las solicitudes de incorporación de cambios dirigidos a `main`.
Los cambios en el comportamiento del servidor también deben verificarse con las pruebas de `cmd` correspondientes
y las pruebas de integración del proyecto de origen. Consulta `Makefile` para conocer sus dependencias y cómo ejecutarlas.

## Alcance de la inicialización

Esta inicialización conserva la ruta del módulo Go original `github.com/minio/minio`, las rutas de importación internas,
la configuración `MINIO_*`, los formatos de datos y el comportamiento en tiempo de ejecución.
El ejecutable puede llamarse `vasca`, aunque la ayuda y la información de versión siguen usando el nombre MinIO.
Los cambios de marca, rutas de módulos y procesos de publicación se realizarán por separado.

La configuración original de GitHub de MinIO se conserva en [docs/upstream/github](docs/upstream/github/)
como referencia para el mantenimiento; vasca utiliza su propio [flujo de CI](.github/workflows/ci.yml).
El `Makefile` y los scripts de publicación originales todavía contienen registros de imágenes, URL de descarga
y destinos de publicación de MinIO. Configura los destinos de este proyecto antes de usar los objetivos de publicación.

## Licencia y procedencia

El código sigue bajo la licencia [GNU AGPL v3](LICENSE). Se conservan los archivos [NOTICE](NOTICE),
[CREDITS](CREDITS) y los avisos de derechos de autor del proyecto de origen en los archivos fuente.
vasca es un proyecto mantenido de forma independiente.

Consulta el [README del commit de referencia](https://github.com/minio/minio/blob/7aac2a2c5b7c882e68c1ce017d8256be2feea27f/README.md)
para ver la documentación original del proyecto de origen.
