# 프로젝트를 진행하면서 겪었던 문제들

## Drawer Navigator

: Drawer Navigator가 nested 되어있는 스크린을 렌더해야할 때
: 드로어 네비게이터의 부모 단에서 드로어 네비게이터를 제어해야할 때

---

### 상황 및 문제

구현해야 하는 프로젝트의 페이지 구조는 다음과 같았다. (문제를 겪었던 일부 구조만 가지고 와보면 ⬇️)

```
- Home (Stack Navigator)
  : headerLeft (메뉴 아이콘)
  : headerTitle (검색바)
  : headerRight (장바구니)
  - Home (Meterial Top Tab Navigator)
    - Home Screen
    - Best (Meterial Top Tab Navigator)
      - All
      - Outer
      - Top
      - ...
    - New Screen
```

여기서 `Home Stack Navigator`의 `headerLeft`(메뉴 아이콘) 을 누르면 드로어 네비게이터가 나타나는데, 문제는 그 드로어 네비게이터는 `Best Meterial Top Tab Navigator`의 하위 카테고리 스크린 (outer, top 등등) 을 렌더한다.

때문에

1. **드로어 네비게이터 안에 상위 네비게이터를 넣자니** (드로어 아이템이 상위 네비게이터를 가리키고 있지 않기 때문에) 상위 네비게이터가 드로어 아이템으로 드러나면 안되고 ( = **드로어 아이템을 보이지 않도록 제공하는 prop이나 options이 없고**)

2. 처음에는 Home Stack Navigator 의 하위요소로 Drawer Navigator를 넣었는데 그렇게 하자니 **부모에서 드로어 네비게이터를 제어하는 방법이 없었다.**

```
If a drawer navigator is nested inside of another navigator that provides some UI, for example a tab navigator or stack navigator, then the drawer will be rendered below the UI from those navigators. The drawer will appear below the tab bar and below the header of the stack. You will need to make the drawer navigator the parent of any navigator where the drawer should be rendered on top of its UI.
```

이하 대충 드로어 네비게이터가 다른 네비게이터의 부모로 와야한다는 내용 _- 리액트 네비게이션 공식문서_

### 이건 내 생각

생각해보면 ~~이걸 구현하다가 문제를 겪었기 때문에 든 생각이기도 하지만~~ 사용자 입장에서도 이런 페이지 흐름은 그닥 좋지 않은 것으로 보인다.

탭으로 이루어진, 즉 병렬로 이루어진 home, best, new 스크린은 각각 all, outer, top 등의 하위 카테고리 탭 메뉴를 가지고 있었다. 하지만 각 페이지에서 메뉴 아이콘을 누르면 똑같은 카테고리의 슬라이드 메뉴가 나타나고 아이콘을 클릭하면 **동일하게 베스트 스크린의 하위 카테고리 스크린으로 이동한다**

이런 구조는

**첫째로**,  
베스트 스크린에서 아주 불필요한 기능이다.  
베스트 스크린에 이미 상단 탭메뉴로 동일한 네비게이션이 있기 때문이다.

**둘째로**,  
다른 스크린(home, new)에서 슬라이드 네비게이션으로 베스트 스크린의 하위 카테고리 페이지로 이동하는 플로우가(병렬적으로 위치한 페이지의 하위 페이지로 이동하는 것) 사용자에게 직관적이지 않다.  
home, new 스크린의 상단 탭 메뉴와 슬라이드 네비게이션이 동일한 항목으로 이루어져 있기 때문이다. (outer, top 등) 똑같은 항목으로 이루어진 네비게이션인데 슬라이드 메뉴는 다른 페이지의 하위 항목을 기리키고 있다.

구현하기 어려워서 하는 말이 아니다 ~~사실 맞다. 하기싫다~~

### 해결방법

1. 드로어 네비게이터 안에 스택 네비게이터를 넣거나
2. 부모 단에서 드로어 네비게이터를 제어할 수 있는 방법을 찾는다.

---

ㅠㅠ 찾았다....
[Drawer + Tab Navigator의 구조](https://www.youtube.com/watch?v=pd01LyE7ts8&t=54s)

요약하면 드로어가 가장 부모위치로 가는게 맞고, (2번의 방법은 없다 !)
드로어 네비게이터의 `drawerContent` prop을 사용하면 되는데, drawerContent의 drawerItem 컴포넌트로 새로 그려줄 카테고리 메뉴를 넣으면 된다. 이 때 `DrawerItemList` prop에 컴포넌트로 Drawer.Screen로 전달해준 정보를 전달해주지 않으면 카테고리 항목으로 보이지 않게 된다.
