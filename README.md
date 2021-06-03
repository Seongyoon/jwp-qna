## 1단계 요구사항

* 아래의 DDL에 맞는 엔티티를 설계한다.

```sql
create table answer
(
    id          bigint generated by default as identity,
    contents    clob,
    created_at  timestamp not null,
    deleted     boolean   not null,
    question_id bigint,
    updated_at  timestamp,
    writer_id   bigint,
    primary key (id)
)

create table delete_history
(
    id            bigint generated by default as identity,
    content_id    bigint,
    content_type  varchar(255),
    create_date   timestamp,
    deleted_by_id bigint,
    primary key (id)
)

create table question
(
    id         bigint generated by default as identity,
    contents   clob,
    created_at timestamp    not null,
    deleted    boolean      not null,
    title      varchar(100) not null,
    updated_at timestamp,
    writer_id  bigint,
    primary key (id)
)

create table user
(
    id         bigint generated by default as identity,
    created_at timestamp   not null,
    email      varchar(50),
    name       varchar(20) not null,
    password   varchar(20) not null,
    updated_at timestamp,
    user_id    varchar(20) not null,
    primary key (id)
)

alter table user
add constraint UK_a3imlf41l37utmxiquukk8ajc unique (user_id)

```

* User, Question, Answer 순으로 엔티티를 매핑한다.
* 각각의 엔티티 매핑을 진행하며 테스트케이스를 작성한다.
* 공통으로 사용하는 컬럼을 분리 가능한지 체크
* 코드 정리

###1단계 질문

Q. 유저 삭제 테스트케이스를 userRepository.delete(JAVAJIGI) 를 실행하여 단독 테스트케이스 실행시에 성공을 했었는데요. 전체 테스트케이스 실행에서는 실패가 되었습니다.  
다른 테스트케이스 실행이 영향을 끼칠 수 있을거 같아서 @BeforeEach 애노테이션으로 userRepository.deleteAll()을 실행하게 하였는데 마찬가지로 테스트케이스는 실패하였습니다.  
원인이 무엇일까요?


## 2단계 요구사항

아래의 DDL을 보고 연관관계 매핑을 한다.

```sql
alter table answer
    add constraint fk_answer_to_question
        foreign key (question_id)
            references question

alter table answer
    add constraint fk_answer_writer
        foreign key (writer_id)
            references user

alter table delete_history
    add constraint fk_delete_history_to_user
        foreign key (deleted_by_id)
            references user

alter table question
    add constraint fk_question_writer
        foreign key (writer_id)
            references user
```

* 연관관계 매핑을 하여 외래키가 제대로 걸리는지 테스트한다.
* 테스트 케이스에서 데이터가 의도한 대로 CRUD 되었는지 확인한다.
