---
title: Typedef struct diffrerence
categories:
- C
feature_image: "https://picsum.photos/2560/600?image=872"
---

Typedef Struct vs Struct
좋아, 이건 C 구조체 문법에서 아주 중요한 개념이야.
typedef struct랑 struct의 차이를 아래처럼 정리할 수 있어:
✅ 차이 요약
문법
사용법
구조체 이름 붙이는 방식
struct Entry {…};
사용할 때 → struct Entry e;
항상 struct 키워드 붙여야
typedef struct {…} Entry;
사용할 때 → Entry e;
struct 키워드 안 써도 됨
typedef struct Entry {…} Entry;
사용할 때 → Entry e;
struct 이름 + 별칭 둘 다 존재
🔍 예제 1:
struct Entry
struct Entry {
    int key;
    int value;
};
void foo() {
    struct Entry e;  // 반드시 struct 붙여야 함
}
🔍 예제 2:
typedef struct {…} Entry
typedef struct {
    int key;
    int value;
} Entry;
void foo() {
    Entry e;  // struct 안 붙여도 됨!
}
🔍 예제 3:
typedef struct Entry {…} Entry
typedef struct Entry {
    int key;
    int value;
} Entry;
void foo() {
    Entry e;              // ✅ 편하게 사용
    struct Entry s;       // ✅ 여전히 struct Entry도 쓸 수 있음
}
이건 이름을 두 개 만들어주는 거야:
struct Entry (구조체 태그)
Entry (typedef 별칭)

✅ 결론
struct Entry는 항상 struct 키워드를 붙여야 함 (좀 귀찮음)
typedef struct {…} Entry; 쓰면 더 간결하게 Entry만 쓰면 돼서 보통 이 방식 선호
구조체가 많아지는 실전에서는 typedef 방식이 훨씬 깔끔하고 편해

필요하면 C 스타일 프로젝트에서 어떤 방식이 더 권장되는지도 정리해줄 수 있어.

<!-- more -->

Retorts have the "cap" and the "cucurbit" made into one. The anbik is also called the raʾs (head) of the cucurbit. The liquid in the cucurbit is heated or boiled; the vapour rises into the anbik, where it cools by contact with the walls and condenses, running down the spout into the receiver. A modern descendant of the alembic is the pot still, used to produce distilled beverages.

_Originally from [Alembic - Wikipedia](https://en.wikipedia.org/wiki/Alembic)_
