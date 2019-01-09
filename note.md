### Vue style guide

* 소문자 기준의 kebab-case(하이픈 구분)로 custom element 작성
  ```
  <list-item></list-item>
  ```
  카멜케이스로 작성해도 작동하는 데에는 이상이 없지만, Vue 내부적으로 camelCase를 kebab-case로 변경해준다. HTML Attributes(속성)이 기본적으로 kebab-case를 따르기 때문에 kebab-case로 작성시 에디터에서 바로 컴포넌트로 연결도 가능하다(에디터 제공). 이렇게 작업 시간도 줄여주므로 권장하는 것은 따르도록 하자.
  
  +) custom component는 자바스크립트에서 변수로 사용되지 않으므로 camelCase를 쓸 이유가 없다는 의견도 있다.

  https://vuejs.org/v2/style-guide/#Self-closing-components-strongly-recommended
  https://www.w3.org/TR/2018/WD-custom-elements-20180216/#html-element-constructors