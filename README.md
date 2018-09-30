# Go Submodules Example

```bash
# create repo
mkdir go-submodules-example
cd go-submodules-example
git init
hub create

# root module
go mod init github.com/veggiemonk/go-submodules-example
git add go.mod
git commit -m "Initial commit"
git push -u origin master

# create a function
mkdir func1
cd func1
cat <<EOF > func1.go
package func1

import (
  "fmt"
  "net/http"

  "github.com/openfaas-incubator/go-function-sdk"
)

// Handle a function invocation
func Handle(req handler.Request) (handler.Response, error) {
  var err error

  message := fmt.Sprintf(">> func1 says: %s", string(req.Body))

  return handler.Response{
    Body:       []byte(message),
    StatusCode: http.StatusOK,
  }, err
}

EOF

# create the module
go mod init github.com/veggiemonk/go-submodules-example/func1
cd ..

# push changes
git add func1
git commit -m 'Add submodule func1'
git push

# tag the module
git tag func1/v0.1.1
git push origin func1/v0.1.1


# create a function
mkdir func1
cd func1
cat <<EOF > func2.go
package func2

import (
  "fmt"
  "net/http"

  "github.com/openfaas-incubator/go-function-sdk"
)

// Handle a function invocation
func Handle(req handler.Request) (handler.Response, error) {
  var err error

  message := fmt.Sprintf(">> func2 says: %s", string(req.Body))

  return handler.Response{
    Body:       []byte(message),
    StatusCode: http.StatusOK,
  }, err
}

EOF

# create the module
go mod init github.com/veggiemonk/go-submodules-example/func2
cd ..


# push changes
git add func2
git commit -m 'Add submodule func1'
git push
# tag release
git tag func2/v0.1.1
git push origin func2/v0.1.1


```

In case of cache problem: `go clean -modcache`