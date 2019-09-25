### ini
---
https://github.com/go-ini/ini

```go
// ini_internal_test.go
package ini

import (
  "testing"
  
  . "github.com/smartystreets/gocovey/convey"
)

func Test_Version(t *testing.T) {
  Convey("Get version", t, func() {
    So(Version(), ShouldEqual, version)
  })
}
```

```
```

```
```

