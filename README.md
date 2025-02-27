<div align="center">

# Issues Notion Sync

</div>

<div align="center">

<a href="https://github.com" title="GitHub">
  <picture>
    <source media="(prefers-color-scheme: dark)" width="128px" height="128px" srcset="images/github-dark.png">
    <img alt="GitHub logo" width="128px" height="128px" src="images/github-light.png">
  </picture>
</a>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<a href="https://notion.com/product" title="Notion">
  <img alt="Notion logo" src="images/notion.png" width="128px" height="128px" />
</a>

</div>

## 1. Requirements

- [x] [**GitHub Token**](https://github.com/settings/tokens) 생성

- [x] [**issues-notion-sync**](https://herokwon.notion.site/1a6ca0268cb380278a7becb09c697ec6?v=1a6ca0268cb380c39bfd000c07e0b778) Notion 템플릿 복제

- [x] Notion [**API 통합**](https://notion.so/profile/integrations) 시크릿 생성  
       :point_right: [Notion API 통합 | Notion 도움말 센터](https://notion.com/ko/help/create-integrations-with-the-notion-api)

<br />

## 2. Settings

- [x] 프로젝트 최상위에 **`.github`** 폴더 위치

- [x] 복제한 **issues-notion-sync** 페이지에 Notion API 연결  
       :point_right: [API 통합 추가와 관리 - 페이지에 연결 추가 | Notion 도움말 센터](https://notion.com/ko/help/add-and-manage-connections-with-the-api?nxtPslug=add-and-manage-connections-with-the-api#%ED%8E%98%EC%9D%B4%EC%A7%80%EC%97%90-%EC%97%B0%EA%B2%B0-%EC%B6%94%EA%B0%80)

- [x] Notion **페이지 ID** 추출  
       :point_right: [Build your first integration - Environment variables | Notion Developers](https://developers.notion.com/docs/create-a-notion-integration#environment-variables)

- [x] GitHub **Repository secrets** 설정
  ```
  GH_TOKEN: GitHub Token
  NOTION_API_KEY: 사용자의 프라이빗 Notion API 통합 시크릿
  NOTION_DATABASE_ID: 사용자가 복제한 GitHub issues 페이지 ID
  ```

<br />

## 3. (Optional) Custom Properties

> **issues-notion-sync** 페이지의 기본 속성과 데이터 형식, 설명 → [Notion API](https://developers.notion.com/reference) 활용 커스텀 가능

|     속성      |        데이터 형식        | 설명                                                    |
| :-----------: | :-----------------------: | :------------------------------------------------------ |
|  **Status**   |      [선택][select]       | Issue 상태(**`Opened`**, **`Closed`**)                  |
|   **Title**   |       [제목][title]       | Issue 제목                                              |
| **Assignees** | [다중 선택][multi-select] | Issue 담당자                                            |
|  **Labels**   | [다중 선택][multi-select] | Issue 레이블 목록                                       |
|   **Date**    |       [날짜][date]        | Issue 생성 ~ 종료 기간 (시간 포함 / TIMEZONE 설정 기준) |
|   **Link**    |        [URL][url]         | Issue 바로가기 링크                                     |

[select]: https://developers.notion.com/reference/page-property-values#select "선택 형식 보기"
[title]: https://developers.notion.com/reference/page-property-values#title "제목 형식 보기"
[multi-select]: https://developers.notion.com/reference/page-property-values#multi-select "다중 선택 형식 보기"
[date]: https://developers.notion.com/reference/page-property-values#date "날짜 형식 보기"
[url]: https://developers.notion.com/reference/page-property-values#url "URL 형식 보기"

```yml
# [.github/workflows/notion.yml]
# 135 ~ 162 Lines

…

body=$(jq --arg title "$ISSUE_TITLE" --arg link "$ISSUE_URL" '. + {
  properties: {
    Status: {
      select: {
        name: "${{ steps.properties.outputs.status }}"
      }
    },
    Title: {
      title: [{
        text: {
          content: $title
        }
      }]
    },
    Assignees: {
      multi_select: ${{ steps.properties.outputs.assignees }}
    },
    Labels: {
      multi_select: ${{ steps.properties.outputs.labels }}
    },
    Date: {
      date: ${{ steps.properties.outputs.date }}
    },
    Link: {
      url: $link
    }
  }
}' body.json)

…
```

<br />

## 4. (Optional) Link to GitHub PR

> GitHub PR - Notion 작업 연동

- [x] Notion - GitHub 워크스페이스 앱 연결 :star:

  1. 사이드바 **`설정`**
  2. '**내 연결**' 클릭 후 **`GitHub (Workspace)`** 찾기
  3. GitHub 계정 및 Repository 연결

  :point_right: [GitHub 통합 - GitHub API 통합 설정 | Notion 도움말 센터](https://notion.com/ko/help/github)

- [x] issues-notion-sync 페이지에서 연동하고자 하는 작업의 **_ID_** 확인

  > (Default) **`ISSUE-*`**

- [x] PR 설명에 **_매직워드_** + **_ID_** 입력

  > ex) **`ref ISSUE-1`**, **`fix ISSUE-1`**, …

  :point_down: <mark>매직워드 종류</mark>

  ```
  · close, closes, closed
  - fix, fixes, fixed
  - resolve, resolves, resolved
  - complete, completes, completed, completing
  - ref, references
  - part of
  - related to
  - contributes to
  - towards
  ```

<br />

:heavy_check_mark: <mark>공식 문서를 보고싶다면?</mark> :point_right: [GitHub 통합 - 매직워드를 사용하여 GitHub PR을 Notion 작업에 링크하기 | Notion 도움말 센터](https://notion.com/ko/help/github)

:heavy_check_mark: <mark>구체적인 예시가 보고싶다면?</mark> :point_right: [샘플 PR](https://github.com/herokwon/issues-notion-sync/pull/1)

<br />

## Reference

- [Notion Developers](https://developers.notion.com)
- [Notion 도움말 센터](https://notion.com/ko/help)
