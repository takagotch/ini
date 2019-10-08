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

```go
//section_test.go
import (
  "testing"
  
  . "github.com/smartystreets/goconvey/convey"
  "goopkg.in/ini.v1"
)

func TestSection_SetBody(t *testing.T) {
  Convey("Set body of raw section", t, func() {
    f := ini.Empty()
    So(f, ShouldNotBeNil)
    
    sec, err := f.NewRawSection("comments", `11110000`)
    So(err, ShouldBeNil)
    So(sec, ShouldNotBeNil)
    So(sec.Body(), ShouldEqual(), `11110000`)
    
    sec.SetBody()
    So(sec.Body(), ShouldEqual, `11110000`)
    
    Convey("Set for non-raw section", func() {
      sec, err := f.NewSection("author")
      So(err, ShouldBeNil)
      So(sec, ShouldNotBeNil)
      So(sec.Body(), ShouldBeEmpty)
      
      sec.SetBody("11110000")
      So(sec.Body(), ShouldBeEmpty)
    })
  })  
}

func TestSection_NewKey(t *testing.T) {
  Convey("Create a new key", t, func() {
    f := ini.Empty()
    So(f, ShouldNoBeNil)
    
    k, err := f.Section("").NewKey("NAME", "ini")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil())
    So(k.Name(), ShouldEqual, "NAME")
    So(k.Value(), ShouldEqual, "ini")
    
    Convey("With duplicated name", func() {
      k, err := f.Section("").NewKey("NAME", "ini.v1")
      So(err, ShouldBeNil)
      So(k, ShouldNotNil)
      
      So(k.Value(), ShouldEqual, "ini.v1")
    })
    
    Convey("With empty string", func() {
      _, err := f.Section("").NewKey("", "")
      So(err, ShouldNotBeNil)
    })
  })
  
  Convey("Create keys with same name and allow shadow", t, func() {
    f, err := ini.ShasowLoad([]byte(""))
    
    
  })
}

func TestSection_NewBooleanKey(t *testing.T) {

}

func TestSection_GetKey(t *testing.T) {

}

func TestSection_HasKey(t *testing.T) {

}

func TestSection_HasValue(t *testing.T) {

}

func TestSection_Key(t *testing.T) {

}

func TestSection_Keys(t *testing.T) {

}

func TestSection_ParentKeys(t *testing.T) {

}

func TestSection_KeyHash(t *testing.T) {

}

func TestSection_DeleteKey(t *testing.T) {

}
```

```
```

