---
title: 코딩 규칙 가이드 
author: daniel.choi
description: "소프트웨어 개발 시에 코딩 가이드를 준수하는 목적은 다수의 개발자들이 상호간의 소스코드에 대한 가독성 및 이해도를 높이고, 해당 표준에 따라 개발함으로써 프로젝트 품질의 일관성을 유지하여 프로젝트 완료 이후의 원활한 시스템 유지보수를 지원할 수 있도록 하는데 있다."
layout: post
---

## Coding style guide 

* 제어문(Control Statement)  
  * if *case 1*
  ``` java
  public boolean isModel(Integer i) {

        : logic 1

      if( i == null) {
          return false;
      } else {

          : logic 2 
          return true;
      }
  }
  ```

  * if *case 2*
  ``` java
  public boolean isModel(Integer i) {
    
      if( i == null) {
          return false;
      }

        : logic 1
        : logic 2
      return true;
  }
  ```
* 메소드 (Method) 
  * if *case 1*
    ``` java
    public void method() {
        for(Model model : ModelList) {
            : 
        }
    }
    ```` 
  * if *case 2*
    ``` java *case 2*
    public void method() {
        for(Model model : ModelList) {
            method2(model);
        }
    }
    private void method2(Model model) {
        :
    }
    ```` 
