<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" width="128px" height="128px" srcset="images/github-dark.png">
  <img alt="Github logo" width="128px" height="128px" src="images/github-light.png">
</picture>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img alt="Notion logo" src="images/notion.png" width="128px" height="128px" />

</div>

<div align="center">

# Issues Notion Sync

</div>

### 1. Requirements

- [x] [**Github Token**](https://github.com/settings/tokens)
- [x] Notion [**Github issues**](https://herokwon.notion.site/173ca0268cb380ea8107cfc86f4f1ef3?v=6a198de8f1d64f7c8724e18f26487ba5) 템플릿
- [x] Notion [**API 통합**](https://notion.so/profile/integrations) 시크릿  
       :point_right: [Notion API 통합 | Notion (노션) 도움말 센터](https://www.notion.com/ko/help/create-integrations-with-the-notion-api)

<br />

### 2. Settings

- [x] 프로젝트 최상위에 **`.github`** 폴더 위치
- [x] 사용자의 **Github issues** 페이지에 Notion API 연결  
       :point_right: [API 통합 추가와 관리 - 페이지에 연결 추가 | Notion (노션) 도움말 센터](https://www.notion.com/ko/help/add-and-manage-connections-with-the-api?nxtPslug=add-and-manage-connections-with-the-api#%ED%8E%98%EC%9D%B4%EC%A7%80%EC%97%90-%EC%97%B0%EA%B2%B0-%EC%B6%94%EA%B0%80)
- [x] **Notion 페이지 ID** 추출  
       :point_right: [Build your first integration - Environment variables | Notion Developers](https://developers.notion.com/docs/create-a-notion-integration#environment-variables)
- [x] Github **Repository secrets** 설정
  ```
  - GH_TOKEN: Github Token
  - NOTION_API_KEY: 사용자의 프라이빗 Notion API 통합 시크릿
  - NOTION_DATABASE_ID: 사용자가 복제한 Github issues 페이지 ID
  ```

<br />

### 3. (Optional) Custom Properties

> **Github issues** 페이지에 적용된 기본 속성과 데이터 형식, 설명  
> (사용자가 임의로 변경하여 활용 가능)

|     속성      |        데이터 형식        | 설명                                          |
| :-----------: | :-----------------------: | :-------------------------------------------- |
|  **Status**   |      [선택][select]       | Issue 상태(**`Opened`**, **`Closed`**)        |
|   **Title**   |       [제목][title]       | Issue 제목                                    |
| **Assignees** | [다중 선택][multi-select] | Issue 담당자                                  |
|  **Labels**   | [다중 선택][multi-select] | Issue 레이블 목록                             |
|   **Date**    |       [날짜][date]        | Issue 생성 ~ 종료 기간 (시간 포함 / UTC 기준) |
|   **Link**    |        [URL][url]         | Issue 바로가기 링크                           |

[select]: https://developers.notion.com/reference/page-property-values#select "선택 형식 보기"
[title]: https://developers.notion.com/reference/page-property-values#title "제목 형식 보기"
[multi-select]: https://developers.notion.com/reference/page-property-values#multi-select "다중 선택 형식 보기"
[date]: https://developers.notion.com/reference/page-property-values#date "날짜 형식 보기"
[url]: https://developers.notion.com/reference/page-property-values#url "URL 형식 보기"

```yml
# [.github/workflows/notion.yml]
# 116 ~ 141 Lines
…
properties: {
  Status: {
    select: {
      name: "${{ steps.properties.outputs.status }}"
    }
  },
  Title: {
    title: [{
      text: {
        content: "$ISSUE_TITLE"
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
    url: "$ISSUE_URL"
  }
}
…
```

<br />

### 4. Try issues!

<br />

## Reference

- [Notion Developers](https://developers.notion.com)
