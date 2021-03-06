# Pull Requests

When creating a pull-request you should:

- __Open an issue first:__ Confirm that the change or feature will be accepted
- __gofmt and vet the code:__ Use  `gofmt`, `golint`, `govet` and `goimports` to clean up your code.
- __Update api documentation:__ If your pull-request adding/modifying an API request, make sure you update the swagger documentation (`api-docs.yml`)

# Installation in a development environment

- [Install Go](https://golang.org/doc/install)
- Install MySQL / MariaDB
- Install node.js

1) Set up the gopath

```
mkdir -p $GOPATH/src/github.com/ansible-semaphore && cd $GOPATH/src/github.com/ansible-semaphore
```

2) Clone semaphore (with submodules)

```
git clone --recursive git@github.com:ansible-semaphore/semaphore.git && cd semaphore
```

3) Install dev dependencies

```
go get ./... github.com/cespare/reflex github.com/jteeuwen/go-bindata/...
npm install async
npm install -g nodemon pug-cli less
```

4) Set up config, database & run migrations

```
cat <<EOT >> config.json
{
    "mysql": {
        "host": "127.0.0.1:3306",
        "user": "root",
        "pass": "",
        "name": "semaphore"
    },
    "port": ":3000"
}
EOT

echo "create database semaphore;" | mysql -uroot -p
go run cli/main.go -config ./config.json -migrate
```

Now it's ready to start.. Run `./make.sh watch`

- Watches js files in `public/js/*` and compiles into a bundle
- Watches css files in `public/css/*` and compiles into css code
- Watches pug files in `public/html/*` and compiles them into html
- Watches go files and recompiles the binary
- Open [localhost:3000](http://localhost:3000)
