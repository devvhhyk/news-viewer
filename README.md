axios : 현재 가장 많이 사용되고 있는 자바스크립트 HTTP 클라이언트이다

이것의 특징은 HTTP 요청을 Promise 기반으로 처리한다는 점이다

```jsx
npm add axios // axios설치
```

```jsx
const onClick = () => {
	axios.get('~~').then(response => {
		setDate(response.data);
	};
};
```

axios.get함수를 사용하였다 이 함수는 파라미터로 전달된 주소에 GET 요청을 해준다

결과는 .then을 통해 확인할 수 있다

```jsx
const onClick = async () => {
	try {
		const response = await axios.get(
			'~~',
		);
		setData(response.data);
	} catch (e) {
		console.log(e);
	}
};
```

화살표함수에 async/await을 적용할 때는 async () ⇒ {}로 적용한다

newsapi에서 API키 발급하기 : https://newsapi.org/register에 가입하여 발급받았다

syeld-components를 사용하여 화면에 보여지는 정보를 만들어보았다

npm add styld-components // 스타일드 컴포넌트 설치

데이터 연동하기

useEffect를 사용하여 컴포넌트가 처음 렌더링되는 시점에 API를 요청하면 된다

이때 useEffect에서 반환해야 하는 값은 뒷정리 함수이기 때문에 useEffect에 async를 붙이면 안된다

useEffect내부에서 async/await를 사용하고 싶다면 함수 내부에 async키워드가 붙은 또 다른 함수를 만들어서 사용해 주어야 한다

리액트 라우터 설치 : npm add react-router-dom

Categories에 NavLink사용

```jsx
const Category = styled(NavLink)`
  font-size: 1.125rem;
  cursor: pointer;
  white-space: pre;
  text-decoration: none;
  color: inherit;
  padding-bottom: 0.25rem;
```

```jsx
const Categories = () => {
  return (
    <CategoriesBlock>
      {categories.map((c) => (
        <Category
          key={c.name}
          className={({ isActive }) => (isActive ? 'active' : undefined)}
          to={c.name === 'all' ? '/' : `/${c.name}`}
        >
          {c.text}
        </Category>
      ))}
    </CategoriesBlock>
  );
};
```

usePromise 커스텀 Hook 만들기

usePromise Hook은 Promise의 대기 중, 완료 결과, 실패 결과에 대햔 상태를 관리하며, usePromise의 의존 배열 deps를 파라미터로 받아온다 받아온 deps배열은 usePromise내부에서 사용한 useEffect의 의존 배열로 설정된다

```jsx
import { useState, useEffect } from 'react';

export default function usePromise(PromiseCreator, deps) {
  // 대기 중/완료/실패에 대한 상태 관리
  const [loading, setLoading] = useState(false);
  const [resolved, setResolved] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    const process = async () => {
      setLoading(true);
      try {
        const resolved = await PromiseCreator();
        setResolved(resolved);
      } catch (e) {
        setError(e);
      }
      setLoading(false);
    };
    process();
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, deps);
  return [loading, resolved, error];
}
```

usePromise를 사용하면 NewsList에서 대기 중 상태 관리와 useEffect를 직접 설정하지 않아도 된다
