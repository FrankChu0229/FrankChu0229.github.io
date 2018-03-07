title: Maven Summary
date: 2018-03-05 12:00:45
tags: [maven, java, package management]
---

## Show all dependencies

- `mvn dependency:tree`

## RM dependnecy in pom

```
      <exclusions>
        <exclusion>
          <groupId>com.group</groupId>
          <artifactId>name</artifactId>
        </exclusion>
      </exclusions>

```

## Reference

- http://wiki.jikexueyuan.com/project/maven/pom.html


---
