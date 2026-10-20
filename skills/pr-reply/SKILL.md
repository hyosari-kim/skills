---
name: pr-reply
description: 현재 repo의 PR 번호를 받아 미해결 리뷰 코멘트에 답글을 단다. 코드는 수정하지 않는다. 사용자가 /pr-reply 로 부를 때만 쓴다.
argument-hint: <PR 번호>
disable-model-invocation: true
allowed-tools: Bash(gh pr view:*), Bash(gh pr comment:*), Bash(gh api:*), Read, Grep, Glob
---

# PR 코멘트 답글

대상 PR: #$ARGUMENTS (현재 repo)

## 0. 준비
Skill 도구로 `superpowers:receiving-code-review`를 먼저 로드하고, 답변할 때 그 원칙을 따른다.
단, 이 skill은 답글만 단다. 코드 수정·커밋·푸시는 하지 않는다.

## 1. 코멘트 수집
- PR 정보: `gh pr view $ARGUMENTS --json number,headRefName,url,author`
- 미해결 리뷰 스레드 (isResolved=false만):
  ```
  gh api graphql -F owner='{owner}' -F repo='{repo}' -F n=$ARGUMENTS -f query='
    query($owner:String!,$repo:String!,$n:Int!){ repository(owner:$owner,name:$repo){
      pullRequest(number:$n){ reviewThreads(first:100){ nodes{
        isResolved path line
        comments(first:50){ nodes{ databaseId author{login} body } } } } } } }'
  ```
- 일반 대화 코멘트: `gh api repos/{owner}/{repo}/issues/$ARGUMENTS/comments`
- 마지막 코멘트가 내가 단 것인 스레드는 건너뛴다. 내 login은 `gh api user --jq .login`로 확인한다.

## 2. 답변 작성
- 답글은 한국어로 쓴다. 원 코멘트가 영어여도 마찬가지다. 코드·식별자·에러 메시지는 원문 그대로 둔다.
- 각 코멘트가 가리키는 파일과 줄을 직접 읽고 맥락을 파악한다.
- 지적이 맞으면 인정하고 어떻게 반영할지 짧게 적는다.
- 지적이 틀렸거나 근거가 부족하면 코드 위치를 들어 설명한다.
- 질문이면 코드를 근거로 답한다. 모르면 추측하지 말고 확인이 필요하다고 적는다.
- 형식적인 감사·동의 문구("좋은 지적입니다!" 등)는 쓰지 않는다.

## 3. 게시
확인 없이 바로 게시한다.
- 인라인 리뷰 코멘트 답글:
  `gh api repos/{owner}/{repo}/pulls/$ARGUMENTS/comments/<databaseId>/replies -f body="..."`
- 일반 코멘트 답글: `gh pr comment $ARGUMENTS --body "..."`

## 4. 보고
코멘트별로 작성자 / 위치 / 원문 요약 / 단 답글을 표로 정리한다.
"반영 필요"로 답한 코멘트는 따로 모아 보여준다.
