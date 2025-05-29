---
title: Typedef struct diffrerence
categories:
- C
feature_image: "https://picsum.photos/2560/600?image=872"
---
# typedef struct vs struct in C

C에서 구조체를 정의하고 사용할 때 `typedef struct`와 `struct`는 다음과 같은 차이가 있습니다.

---

## ✅ 차이 요약

| 항목 | struct Entry | typedef struct { ... } Entry | typedef struct Entry { ... } Entry |
|------|---------------|-------------------------------|------------------------------------|
| 선언 방식 | `struct Entry e;` | `Entry e;` | `Entry e;` 또는 `struct Entry e;` |
| struct 키워드 필요 | 항상 필요 | 불필요 | 선택 가능 |
| 이름 정의 | 구조체 태그만 정의 | 별칭만 정의 | 구조체 태그 + 별칭 모두 정의 |

---

## 🔍 예제들

### struct Entry
```c
struct Entry {
    int key;
    int value;
};

void foo() {
    struct Entry e;  // 반드시 struct 키워드 필요
}
```

### typedef struct { ... } Entry
```c
typedef struct {
    int key;
    int value;
} Entry;

void foo() {
    Entry e;  // struct 키워드 없이 바로 사용 가능
}
```

### typedef struct Entry { ... } Entry
```c
typedef struct Entry {
    int key;
    int value;
} Entry;

void foo() {
    Entry e;        // typedef 별칭으로 사용
    struct Entry s; // struct 키워드로도 사용 가능
}
```

---

## ✅ 결론

- `struct Entry`는 매번 `struct`를 붙여야 해서 **불편**합니다.
- `typedef struct { ... } Entry;`는 `Entry`만으로 간단하게 사용할 수 있어 **실전에서 선호**됩니다.
- `typedef struct Entry { ... } Entry;`는 태그와 별칭을 **둘 다 사용**하고 싶을 때 유용합니다.
- 규모가 큰 프로젝트나 협업에서는 **typedef 방식이 일반적으로 더 깔끔하고 가독성도 좋습니다**.


