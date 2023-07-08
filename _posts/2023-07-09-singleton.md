---
title:  "싱글톤 패턴"
date: '2023-07-09 05:09:00 +09:00'
category: ["design-pattern"]
tags: ["design-pattern", "디자인패턴", "싱글톤패턴"]
---

# 생성패턴 - Sigleton

---

> 오늘은 싱글턴패턴에 대해서 공부하고
> 
> 
> 어떤 문제가 생겼을때 싱글턴패턴으로 해결 할 수 있는지 알아보자!
> 

## ❗️해결하려는 문제

---

- 클래스의 인스턴스가 단 1개임을 보장해야 한다.
- 인스턴스 생성을 제어할 수 있어야 한다.
- 1개뿐인 인스턴스에 쉽게 접근할 수 있어야한다 (글로벌)

## 클래스 다이어그램

---

![singleton](/assets/img/posts/singleton.png)

클래스다이어그램에서 **-**는 private,  **+**는 public을 의미한다

이처럼  객체를 필드로 가지고있고 생성자를 private으로 막은 뒤 

getInstance 메서드로만 접근 가능하도록 설계 되어있다

다음은 코드로 알아보자.

## 구현 코드

싱글턴 방식의 구현코드에는 두가지정도가 있을 수 있는데

제일 간단한 코드는 아래와 같이 생성시점에 초기화를 바로 하는 방법이다

```java
public class Singleton {

    private static Singleton instance = new Singleton();

    private Singleton() {
    }

    public static Singleton getInstance() {
        return instance;
    }
}
```

하지만 이 코드에는 instance를 사용하지 않더라도 메모리에 올라가는 단점이 있다

이를 보완하기 위해서 초기화를 지연하는 방법인데

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

이런식으로 **getInstance()**를 호출하는 시점에 instance가 null인지 확인하고

instance를 초기화해주는 방법을 사용할 수 있다.

하지만 여기서 또하나의 문제가 발생할 수 있다.

instance가 null인지 확인하는 코드가 **thread safe** 하지 않다는 것이다.

만약 두개의 쓰레드가 동시에 저 코드에 진입했다면 instance가 null이기 때문에

두번의 **new Singleton()** 을 할 것이다.

이것을 해결하기 위해 코드를 이렇게 변경하면 된다.

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
    }

    private static class SingletonHolder {
        private static final Singleton instance = new Singleton();
    }
    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
}
```

위 방식은 **"Bill Pugh Singleton”** 이라는 방식이라고 한다.

**`Singleton.getInstance()`** 메서드가 호출되는 시점에서 **`SingletonHolder`** 클래스가 처음으로 사용되며, 이때 **`SingletonHolder`** 클래스가 로딩되고 정적 필드인 **`instance`**가 초기화된다 따라서 인스턴스는 **`Singleton.getInstance()`** 메서드가 처음 호출될 때 생성되고, 그 후에는 이미 생성된 인스턴스를 반환하기
때문에 인스턴스를 지연 초기화하고 스레드 안전성을 보장할 수 있다.
