<div align="center">
  <img src="https://github.com/g6y116/PersonalProject/assets/121198194/cace1d2c-9a2c-4321-a85c-df69b98358c1" width="64px">
  <H1>모두의 소설 공간 - Noveling</H1>
</div>

# 배포 정보

서비스 링크

[모두의 소설 공간 - Noveling](http://ec2-13-209-207-54.ap-northeast-2.compute.amazonaws.com)

테스트 계정 정보
- ID : qwer
- PW : qwerqwer

# 소개

- 소설을 올리고 공유하는 사이트입니다.
- 학생 시절 PHP로 만들었던 기획을 가져와 Spring으로 구현하였습니다.

<table>
  <tr>
    <td align="center">
      <img src="https://github.com/g6y116/Noveling/assets/121198194/e820abbf-1318-4af6-b03b-6ec86a515618" width="400px">
      <figcaption><p>학생 시절 PHP로 만든 사이트</p></figcaption>
    </td>
    <td align="center">
      <img src="https://github.com/g6y116/PersonalProject/assets/121198194/18f458c1-3479-49a2-a2ba-928577ab4a96" width="400px">
      <figcaption><p>현재 호스팅 사이트</p></figcaption>
    </td>
  </tr>
</table>

# 아키텍처 정보

<img src="https://github.com/g6y116/Noveling/assets/121198194/4a05d751-5644-4703-af85-f47376580de1" width="800px">

# 프로젝트 정보

- Java 11, SpringBoot 2.7.17, Mysql 8.0.35
- Thymeleaf, Spring Data JPA, QueryDsl, SpringSecurity, Lombok, Bootstrap 5.0
- AWS EC2, AWS RDS, Docker, Github Action

# 변경 사항

- 엔티티명 변경 : User -> Member
  - User가 DB에서 예약어로 지정되어 있어 변경

- 엔티티명 변경 : Page -> Chapter
  - org.springframework.data.domain.Page와 클래스명이 겹쳐서 변경
  - 코틀린에서는 임포트 부분에 as로 별칭을 줄 수 있으나 자바는 없어서 아쉬웠음.

- 쿼리 dsl 코드의 부피가 크지 않아 분리된 레포지토리 클래스로 작성


# 패키지 구조

```
📦sj
 ┗ 📂noveling
 ┃ ┣ 📂controller
 ┃ ┃ ┣ 📜ChapterController.java
 ┃ ┃ ┣ 📜CommentController.java
 ┃ ┃ ┣ 📜HomeController.java
 ┃ ┃ ┗ 📜NovelController.java
 ┃ ┣ 📂dto
 ┃ ┃ ┣ 📜ChapterDetailDto.java
 ┃ ┃ ┣ 📜ChapterSimpleDto.java
 ┃ ┃ ┣ 📜CommentDto.java
 ┃ ┃ ┣ 📜NovelDetailDto.java
 ┃ ┃ ┗ 📜NovelSimpleDto.java
 ┃ ┣ 📂entity
 ┃ ┃ ┣ 📜Chapter.java
 ┃ ┃ ┣ 📜Comment.java
 ┃ ┃ ┣ 📜Member.java
 ┃ ┃ ┗ 📜Novel.java
 ┃ ┣ 📂exception
 ┃ ┃ ┣ 📜DataExistsException.java
 ┃ ┃ ┣ 📜DataNotFoundException.java
 ┃ ┃ ┗ 📜NoPermissionException.java
 ┃ ┣ 📂form
 ┃ ┃ ┣ 📜AddChapterForm.java
 ┃ ┃ ┣ 📜AddCommentForm.java
 ┃ ┃ ┣ 📜AddNovelForm.java
 ┃ ┃ ┣ 📜JoinForm.java
 ┃ ┃ ┣ 📜LoginForm.java
 ┃ ┃ ┣ 📜SetChapterForm.java
 ┃ ┃ ┣ 📜SetCommentForm.java
 ┃ ┃ ┗ 📜SetNovelForm.java
 ┃ ┣ 📂repository
 ┃ ┃ ┣ 📜ChapterRepository.java
 ┃ ┃ ┣ 📜CommentRepository.java
 ┃ ┃ ┣ 📜MemberRepository.java
 ┃ ┃ ┣ 📜NovelRepository.java
 ┃ ┃ ┗ 📜QueryDslRepository.java
 ┃ ┣ 📂service
 ┃ ┃ ┣ 📜ChapterService.java
 ┃ ┃ ┣ 📜CommentService.java
 ┃ ┃ ┣ 📜MemberService.java
 ┃ ┃ ┣ 📜NovelService.java
 ┃ ┃ ┗ 📜SecurityService.java
 ┃ ┣ 📂type
 ┃ ┃ ┣ 📜Genre.java
 ┃ ┃ ┗ 📜Grade.java
 ┃ ┣ 📜Const.java
 ┃ ┣ 📜NovelingApplication.java
 ┃ ┗ 📜SecurityConfig.java
```

# 핵심 기능

### 로그인/회원가입

- 스프링 시큐리티를 사용하여 기능을 구현하였습니다.

<img src="https://github.com/g6y116/PersonalProject/assets/121198194/3cc0d0ed-8497-4acd-af3b-0aa7ce915bed" width="40%">

<img src="https://github.com/g6y116/PersonalProject/assets/121198194/9e4ce35f-400f-4f7f-8d81-4d1faf300d41" width="40%">

SecurityConfig.java
```
@Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests((authorizeHttpRequests) -> authorizeHttpRequests.requestMatchers(new AntPathRequestMatcher("/**")).permitAll())
            .csrf((csrf) -> csrf.ignoringRequestMatchers(new AntPathRequestMatcher("/h2-console/**")))
            .headers((headers) -> headers.addHeaderWriter(new XFrameOptionsHeaderWriter(XFrameOptionsHeaderWriter.XFrameOptionsMode.SAMEORIGIN)))
            .formLogin((formLogin) -> formLogin.loginPage("/login").defaultSuccessUrl("/"))
            .logout((logout) -> logout.logoutRequestMatcher(new AntPathRequestMatcher("/logout")).logoutSuccessUrl("/").invalidateHttpSession(true))
        ;
        return http.build();
    }
```

### 소설 조회 및 페이징

- 장르별 / 검색어로 조회할 수 있습니다.
- 페이징 처리를 구현 하였습니다.
- QueryDsl로 동적 쿼리를 구현하였습니다.

<img src="https://github.com/g6y116/PersonalProject/assets/121198194/484017b6-f0d6-4871-b618-df2186a2032f" width="80%">

QueryDslRepository.java
```
public Page<Novel> pageNovels(String query, Genre genre, Pageable pageable) {
    QueryResults<Novel> results = queryFactory
            .selectFrom(novel).join(novel.member, member)
            .where(queryEq(query), genreEq(genre))
            .orderBy(novel.id.desc())
            .offset(pageable.getOffset())
            .limit(pageable.getPageSize()).fetchResults();

    List<Novel> content = results.getResults();
    long total = results.getTotal();

    return new PageImpl<>(content, pageable, total);
}

private BooleanExpression queryEq(String query) {
    return query != null ? (novel.title.contains(query)).or(novel.member.name.contains(query)) : null;
}

private BooleanExpression genreEq(Genre genre) {
    return genre != null ? novel.genre.eq(genre) : null;
}
```

### 소설, 회차, 댓글 CRUD

- 빈 유효성 검사를 적용하였습니다.
- 권한이 없는 경우 리다이렉트 처리하였습니다.
- 예외 발생 시 오류페이지를 뛰우도록 하였습니다.
- 서비스 계층에서 DTO 반환 후 컨트롤러에서 뷰에 데이터를 넘겨주도록 하였습니다.
- 작성자 본인인 경우 추가적인 작업이 가능하도록 구현하였습니다.

<img src="https://github.com/g6y116/PersonalProject/assets/121198194/fd1f9eed-617d-4601-a649-e13ec6739100" width="80%">

<img src="https://github.com/g6y116/PersonalProject/assets/121198194/e7186e62-425b-4566-994d-316403e67f4f" width="80%">

Novel.java (Entity)
```
public NovelDetailDto toNovelDetailDto() {
    ...
    return new NovelDetailDto(
            ...
            reverseChapters.stream().map(Chapter::toChapterSimpleDto).collect(Collectors.toList())
    );
}
```

NovelService.java (Service)
```
public NovelDetailDto getNovel(Long novelId) {
    Optional<Novel> originResult = novelRepository.findById(novelId);

    if (originResult.isPresent()) {
        originResult.get().setViewCount(originResult.get().getViewCount() + 1); // 조회 수 처리
        novelRepository.save(originResult.get());
        return originResult.get().toNovelDetailDto();
    } else {
        throw new DataNotFoundException("작품이 존재하지 않습니다.");
    }
}
```

AddNovelForm.java (Form)
```
public class AddNovelForm {

  @NotEmpty(message = "필수 입력 항목입니다.")
  @Size(max=64, message = "64 글자 이내로 입력해주세요.")
  private String title;

  ...
}
```

### 인기 작품 / 최신 작품 추천

- 인기 작품 : 조회 수 기반으로 가중치를 주어 판단하였습니다.
- 최신 작품 : 소설 정보 변경, 회차 정보 생성/변경 시간을 기반으로 판단하였습니다.

<img src="https://github.com/g6y116/PersonalProject/assets/121198194/5e1285ec-8e99-4968-83c9-750dff963ca6" width="30%">

<img src="https://github.com/g6y116/PersonalProject/assets/121198194/4ecfa084-6c82-4a4c-a8ad-a54fd55e6095" width="30%">

NovelService.java (Service)
```
public List<NovelSimpleDto> getNewestNovels() {
    List<Novel> originResults = queryDslRepository.newestNovels();

    return originResults.stream().map(Novel::toNovelSimpleDto).collect(Collectors.toList());
}

public List<NovelSimpleDto> getBestNovels() {
    List<Novel> originResults = queryDslRepository.bestNovels();

    return originResults.stream().map(Novel::toNovelSimpleDto).collect(Collectors.toList());
}
```

QueryDslRepository.java (Repository)
```
// 최근 작품 리스트
public List<Novel> newestNovels() {
    QueryResults<Novel> results = queryFactory
            .selectFrom(novel)
            .where(novel.modifyDate.isNotNull())
            .orderBy(novel.modifyDate.desc())
            .limit(3)
            .fetchResults();

    List<Novel> contents = results.getResults();
    return contents;
}

// 인기 작품 리스트
public List<Novel> bestNovels() {
    QueryResults<Novel> results = queryFactory
            .selectFrom(novel)
            .where(novel.viewCount.ne(0))
            .orderBy(novel.viewCount.desc())
            .limit(3)
            .fetchResults();

    List<Novel> contents = results.getResults();
    return contents;
}
```
