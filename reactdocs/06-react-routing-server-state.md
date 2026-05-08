# 라우팅, 서버 상태, API(Application Programming Interface) 연동

실제 React 앱은 여러 페이지, API(Application Programming Interface) 요청, 캐시, 권한, 에러 상태를 다룹니다. 이 장은 화면 전환과 서버 데이터 흐름을 정리합니다.

## 라우팅

SPA(Single Page Application)는 브라우저 전체 페이지를 새로 받지 않고 클라이언트에서 화면을 전환합니다.

일반적인 라우트:

- `/login`
- `/posts`
- `/posts/:postId`
- `/posts/new`
- `/settings`

라우팅을 설계할 때는 URL(Uniform Resource Locator)이 현재 화면 상태를 설명하도록 만듭니다.

## URL 파라미터

게시글 상세:

```jsx
function PostPage() {
  const { postId } = useParams();
  return <PostDetail postId={postId} />;
}
```

ID는 문자열로 들어오므로 숫자가 필요하면 변환과 검증을 합니다.

## query string

검색, 정렬, 페이지 번호는 query string에 적합합니다.

```text
/posts?keyword=spring&page=2&sort=latest
```

장점:

- 새로고침해도 조건이 유지됩니다.
- 링크 공유가 가능합니다.
- 뒤로 가기와 앞으로 가기가 자연스럽습니다.

## 서버 상태

서버 상태는 클라이언트가 소유하지 않는 데이터입니다.

특징:

- 네트워크 요청이 필요합니다.
- 로딩과 에러가 있습니다.
- 다른 사용자가 바꿀 수 있습니다.
- 캐시 만료가 필요합니다.

사용자 입력 중인 값과 서버에서 온 데이터를 같은 방식으로 관리하면 복잡해집니다.

## API(Application Programming Interface) 클라이언트 분리

나쁜 예:

```jsx
useEffect(() => {
  fetch("/api/posts")
    .then((response) => response.json())
    .then(setPosts);
}, []);
```

개선:

```javascript
export async function getPosts(params) {
  const response = await fetch(`/api/posts?${params}`);
  if (!response.ok) {
    throw new ApiError(response.status, await response.json());
  }
  return response.json();
}
```

화면은 API 요청 세부사항보다 상태 표현에 집중합니다.

## 로딩, 에러, 빈 상태

```jsx
if (isLoading) {
  return <PostListSkeleton />;
}

if (error) {
  return <ErrorPanel message="게시글을 불러오지 못했습니다." />;
}

if (posts.length === 0) {
  return <EmptyPosts />;
}

return <PostList posts={posts} />;
```

로딩, 에러, 빈 상태는 예외 케이스가 아니라 사용자 경험의 기본 흐름입니다.

## mutation

서버 데이터를 변경하는 요청입니다.

- 생성
- 수정
- 삭제
- 좋아요
- 파일 업로드

mutation 후에는 관련 목록과 상세 데이터를 다시 가져오거나 캐시를 갱신해야 합니다.

## 낙관적 업데이트

서버 응답 전에 UI를 먼저 바꾸는 방식입니다.

예시:

- 좋아요 버튼
- 체크박스 상태
- 간단한 즐겨찾기

주의:

- 실패 시 롤백해야 합니다.
- 결제, 재고, 권한처럼 중요한 데이터에는 신중해야 합니다.
- 서버와 클라이언트 상태가 어긋나는 상황을 처리해야 합니다.

## 인증 상태

인증 상태는 아래를 구분해야 합니다.

- 아직 확인 중
- 로그인됨
- 로그인되지 않음
- 토큰 만료
- 권한 부족

```jsx
if (auth.status === "checking") {
  return <LoadingScreen />;
}

if (auth.status === "anonymous") {
  return <Navigate to="/login" />;
}
```

## 체크리스트

- [ ] URL이 현재 화면 상태를 설명한다.
- [ ] 서버 상태와 로컬 UI 상태를 구분한다.
- [ ] API 호출 코드를 화면에서 분리한다.
- [ ] mutation 후 데이터 갱신 전략이 있다.
- [ ] 인증 확인 중과 미로그인 상태를 구분한다.
