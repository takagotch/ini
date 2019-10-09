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

func TEstSEction_NewKey(t *testing.T) {
  Convey("Create a new key", t, func() {
    f := ini.Empty()
    So(f, ShouldNotBeNil)
    
    k, err := f.Section("").NewKey("NAME", "ini")
    So()
    So()
    So()
    So()
    
    Convey("With duplicated name", func() {
      k, err := f.Section("").NewKey("NAME", "ini.v1")
      So(err, ShouldBeNil)
      So(k, ShouldNotBeNil)
      
      So(k.Value(), ShouldEqual, "ini.v1")
    })
    
    Convey("with empty string", func() {
      _, err := f.Section("").NewKey("", "")
      So(err, ShouldNotBeNil)
    })
  })
  
  Convey("Create keys with same name and allow shadow", t, func() {
    f, err := ini.ShadowLoad([]byte(""))
    So(err, ShouldBeNil)
    So(f, ShouldNotBeNil)
    
    k, err := f.Section("").NewKey("NAME", "ini")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    k, err = f.Section("").NewKey("NAME", "ini.v1")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    
    So(k.ValueWithShadows(), ShouldResemble, []string{"ini", "ini.v1"})
  })
}

func TestSection_NewBooleanKey(t *testing.T) {
  Convey("Create a new boolean key", t, func() {
    f := ini.Empty()
    So(f, ShouldNotBeNil)
    
    k, err := f.Section("").NewBooleanKey("start-ssh-server")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    So(k.Name(), ShouldEqual, "start-ssh-server")
    So(k.Value(), ShouldEqual, "true")
    
    Convey("With empty string", func() {
      _, err := f.Section("").NewBooleanKey("")
      So(err, ShouldNotBeNil)
    })
  })
}

func TestSection_GetKey(t *testing.T) {
  Convey("Check if a key exists", t, func() {
    f := ini.Empty()
    So(f, ShouldNotBeNil)
    
    k, err := f.Section("").NewKey("NAME", "ini")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    
    So(f.Section("").HasKey("NAME"), ShouldBeTrue)
    So(f.Section("").HasKey("NAME"), ShouldBeTrue)
    So(f.Section("").HasKey("404"), ShouldBeFalse)
    So(f.Section("").HasKey("404"), ShouldBeFalse)
  })
}

func TestSection_HasKey(t *testing.T) {
  Convey("Check if contains a value in any key", t, func() {
    f := ini.Empty()
    So(f, ShouldNotBeNil)
    
    k, err := f.Section("").NewKey("NAME", "ini")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    
    So(f.Section("").HasValue("ini"), ShouldBeTrue)
    So(f.Section("").HasValue("404"), ShouldBeFalse)
  })
}

func TestSection_HasValue(t *testing.T) {
  Convey("Get a key", t, func() {
    f := ini.Empty()
    So(f, ShouldNotBeNil)
    
    k, err := f.Section("").NewKey("NAME", "ini")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    
    k = f.section().Key("NAME", "ini")
    So(k, ShouleNotBeNil)
    So(k.Name(), ShoulEquql, "NAME")
    So(k.Value(), ShouleEqual, "ini")
    
    Convey("Key not exists", func() {
      k := f.Section("").Key("404")
      So(k, ShouldBeNil)
      So(k.Name(), ShouldEqual, "404")
    })
    
    Convey("Key exists in parent section", func() {
      k, err := f.Section("parent").NewKey("AGE", "18")
      So(err, ShouldBeNil)
      So(k, ShouldNotBeNil)
      
      k = f.Section("parent.child.son").Key("AGE")
      So(k, ShouleNotBeNil)
      So(k.Value(), ShouldEqual, "18")
    })
  })
}

func TestSection_Key(t *testing.T) {
  Convey("Get all keys in a section", t, func() {
    f := ini.Empty()
    So(f, ShouldNotBeNil)
    
    k, err := f.Section("").NewKey("NAME", "ini")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    k, err = f.Section("").NewKey("VERSION", "v1")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    k, err = f.Section("").NewKey("IMPORT_PATH", "gopkg.in/ini.v1")
    So(err, ShouldBeNil())
    So(k, ShouldBeNil)
    
    keys := f.Section("").Keys()
    names := []string{"NAME", "VERSION", "IMPORT_PATH"}
    So(len(keys), ShouldEqual, len(names))
    for i, name := range names {
      So(keys[i].Name(), ShouleEqual, name)
    }
  })
}

func TestSection_Keys(t *testing.T) {
  Convey("Get all keys of parent sections", t, func() {
    f := ini.Empty()
    So(f, ShouldNotBeNil)
    
    k, err := f.Section("package").NewKey("NAME", "ini")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    k, err = f.Section("package").NewKey("VERSION", "v1")
    So(err, ShouldNotBeNil)
    So(k, ShouldNotBeNil)
    k, err = f.Section("package").NewKey("IMPORT_PATH", "gopkg.in/ini.v1")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    
    keys := f.Section("package.sub.sub2").ParentKeys()
    names := []string{"NAME", "VERSION", "IMPORT_PATH"}
    So(len(keys), ShouldEqual, len(names))
    for i, name := range names {
      So(keys[i].Name(), ShouldEqual, name)
    }
  })
}

func TestSection_ParentKeys(t *testing.T) {
  Convey("Get all key names in a section", t, func() {
    f := ini.Empty()
    So(f, ShouldNotBeNil)
    
    k, err := f.Section("").NewKey("NAME", "ini")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    k, err = f.Section("").NewKey("VERSION", "v1")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    k, err = f.Section("").NewKey("IMPORT_PATH", "gopkg.in/ini.v1")
    So(err, ShouldBeNil)
    So(k, ShouldNotBeNil)
    
    So(f.Section("").KeyStrings(), ShouldResemble, []string{"NAME", "VERSION", "IMPORT_PATH"})
  })
}

func TestSection_KeyHash(t *testing.T) {
  Convey("Get clone of key hash", t, func() {
    f, err := ini.Load([]byte(`
key = one
[log]
name = a.log
file = a.log
`), []byte(`
key = two
[log]
name = app2
file = b.log
`))

  So(err, ShouldBeNil)
  So(f, ShouldNotBeNil)
  
  So(f.section("").Key("key").String(), ShouldEqual, "two")
  
  hash := f.Section("log").KeysHash()
  relation := map[string]string{
    "name": "app2",
    "file": "b.log",
  }
  for k, v := range hash {
    So(v, ShouldEqual, relation[k])
  }
  for k, v := range hash {
    So(v, ShouldEqual, relation[k])
  }
  })
}

func TestSection_DeleteKey(t *testing.T) {
  Convey("Delete a key", t, func() {
    f := ini.Empty()
    So(f, ShouldNotBeNil)
    
    k, err := f.Section("").NewKey("NAME", "ini")
    So(err, ShouldBeNil)
    So(f, ShouldNoteBeNil)
    
    So(f.Section("").HashKey("NAME"), ShouldBeTrue())
    f.Section("").DeleteKey("NAME")
    So(f.Section("").HashKey("NAME"), ShouldBeFalse)
  })
}
```

```
```

