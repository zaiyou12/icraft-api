---
title: ICRAFT API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='https://www.icraft21.com'>ICRAFT Homepage</a>
  - Made by <a href='https://blackruby.studio'>BlackRubyStudio</a>
  - info@blackrubystudio.com

includes:
  - errors

search: true
---

# Introduction

## ICRAFT API

> API Endpoint

```shell
https://api.icraft21.com/prod/
```

ICRAFT API는 아이크래프트 홈페이지와 연동하여 뉴스/주가/공시 정보/채용 정보등을 제공하기 위한 API입니다. 아이크래프트에 관심이 있고 소식을 받아 보고 싶으신 분들을 위하여 API 문서를 공개합니다.

API는 사용하기 최대한 용이하게 설계되어 있어, 각 object의 parameter만 파악하시면 쉽게 사용하실 수 있습니다.

좌측에는 호출하는 함수와 함수에 필요한 변수를 표시했으며, 우측에는 javascript를 이용해서 웹에서 호출할 수 있는 코드 예시를 보여줍니다.

문서상이나 API 자체에 문제가 있거나 건의사항이 있을 경우 언제든 `info@blackrubystudio.com`이나 [Github repository](https://github.com/zaiyou12/icraft-api)로 문의 주세요. 

# Authentication

> 구체적인 방법은 SDK와 토큰을 발급 받을 시, 코드 예시와 주석을 참고해주세요.

정보를 받아보기 위한 `GET` 메소드에서는 특별한 인증 절차가 없습니다. 고객소리방의 문의하기 기능은 `POST` 메소드들중 유일하게 인증 절차가 없고, 그 외 모든 `POST` 메소드들은 별도의 인증 절차가 있습니다.

<aside class="warning">
ICRAFT API는 인증 받은 사용자만 SDK와 access token를 발급받아 사용할 수 있습니다. 계정등록을 원하신다면 <strong>info@blackrubystudio.com</strong>으로 연락주시기 바랍니다. 
</aside>

# Disclosure

## Get

> Example Request

```javascript
$.ajax({
  method: 'GET',
  url: 'https://api.icraft21.com/prod/disclosure?page=' + pageNum + '&search' + searchString,
  contentType: 'application/json',
  success: completeRequest,
  error: errorRequest
});
```

> Example Response:

```json
{
  "statusCode": 200,
  "body": {
    "result": [
      {
        "id": 67,
        "title": "분기보고서 (2018.03)",
        "date": "2018-05-14",
        "submitter": "아이크래프트",
        "info_url": "http://dart.fss.or.kr/dsaf001/main.do?rcpNo=20180514005425"
      },
      {
        "id": 66,
        "title": "연결재무제표기준 영업(잠정)실적(공정공시)",
        "date": "2018-05-10",
        "submitter": "아이크래프트",
        "info_url": "http://dart.fss.or.kr/dsaf001/main.do?rcpNo=20180510900661"
      }
    },
    "total": 67
  }
}
```

공시 정보(disclosure)는 투자에 올바른 판단을 하실 수 있도록 제공되는 아이크래프트의 공시정보입니다. 공시정보중 `info_url`은 전자공시 사이트인 [다트](https://dart.fss.or.kr/)로 연결됩니다. 

### Query String

Parameter | Type | Description
---------|-----------|-----------
page | optional, default=1 | 페이지 수, 10개 단위로 페이지가 구분됩니다.
search | optional | 검색어와 `title`을 비교하여, 검색어가 포함된 경우 결괏값을 리턴합니다.

### Returns

Parameter | Description
--------- |-----------
title | 제목
date | 시간
submitter | 제출인
info_url | 공시 정보 링크
total | 전체 `Disclosure object` 갯수


## POST

> Example Request

```javascript
$.ajax({
  method: 'POST',
  url: 'https://api.icraft21.com/prod/disclosure',
  data: JSON.stringify({
    "title": title,
    "submitter": submitter,
    "info_url": info_url
  }),
  contentType: 'application/json',
  success: completeRequest,
  error: errorRequest
});
```

> Example Response:

```json
{
	"statusCode": 201
}
```

공시 정보 등록. `POST` 메소드는 등록된 사용자만 `access token`을 가지고 사용 할수 있으며, 위 `Authentication` 내용을 참고 부탁드립니다.  

### Query String

Parameter | Type | Description
--------- |----------- |-----------
title | string | 공시 제목
submitter | string | 제출인
info_url | url | 공시 정보 링크
disclosure_id | optional, default=-1 | 기본값으로 보내거나 포함시키지 않을 경우 자동으로 신규 object가 생성됩니다. id가 보내진다면 해당 내용으로 업데이트합니다.


## DELETE

> Example Request

```javascript
$.ajax({
  method: 'POST',
  url: 'https://api.icraft21.com/prod/delete',
  data: JSON.stringify({
    "object_id": 1,
    "object_name": "disclosure",
  }),
  contentType: 'application/json',
  success: completeRequest,
  error: errorRequest
});
```

> Example Response:

```json
{
	"statusCode": 200
}
```

공시 정보 삭제. `DELETE` 메소드는 등록된 사용자만 `access token`을 가지고 사용 할수 있으며, 위 `Authentication` 내용을 참고 부탁드립니다.  

### Query String

Parameter | Type | Description
--------- |----------- |-----------
object_id | int | 아이디 값
object_name | string | object 이름


# News

## GET

> Example Request

```javascript
$.ajax({
  method: 'GET',
  url: 'https://api.icraft21.com/prod/news?page=' + pageNum + '&search' + searchString,
  contentType: 'application/json',
  success: completeRequest,
  error: errorRequest
});
```

> Example Response:

```json
{
  "statusCode": 200,
  "body": {
    "result": [
      {
        "id": 51,
        "title": "아이크래프트,정품∙가짜 99%인식하는 최첨단 위변조방지 솔루션 세계 첫 상용화",
        "date": "2018-06-09",
        "count": "90"
      },
      {
        "id": 49,
        "title": "올해 주총 신규사업 핫 아이템은 암호화폐",
        "date": "2018-03-09",
        "count": "590"
      }
    },
    "total": 51
  }
}
```

News는 아이크래프트 보도 자료 소식입니다.

### Query String

Parameter | Type | Description
---------|-----------|-----------
page | optional, default=1 | 페이지 수, 10개 단위로 페이지가 구분됩니다.
search | optional | 검색어와 `title`을 비교하여, 검색어가 포함된 경우 결괏값을 전달합니다.
news_id | optional | 1개의 개별 `object`에 관한 세부 정보를 보고 싶을 경우, `id`를 전송하면 기사 세부 내용을 전달합니다.

### Returns

Parameter | Description
--------- |-----------
id | 번호
title | 제목
date | 시간
count | 조회수
total | 전체 `News object` 갯수
body | 기사 내용 (article로 호출할 경우만 리턴)

## POST

> Example Request

```javascript
$.ajax({
  method: 'POST',
  url: 'https://api.icraft21.com/prod/news,
  data: JSON.stringify({
    "title": title,
    "body": body,
    "date": "2018-06-13",
  }),
  contentType: 'application/json',
  success: completeRequest,
  error: errorRequest
});
```

> Example Response:

```json
{
  "statusCode": 201,
}
```

아이크래프트 보도 자료 등록. `POST` 메소드는 등록된 사용자만 `access token`을 가지고 사용 할수 있으며, 위 `Authentication` 내용을 참고 부탁드립니다. 

### Query String

Parameter | Type | Description
--------- |-----------| -----------
news_id | optional, default=-1 | 기본값으로 보내거나 포함시키지 않을 경우 자동으로 신규 object가 생성됩니다. id값이 보내진다면 해당 내용으로 업데이트합니다.
title | string | 제목
body | html |기사 내용
date | optional, default=now(), date | 날짜, 형식: '%Y-%m-%d'


## DELETE

> Example Request

```javascript
$.ajax({
  method: 'POST',
  url: 'https://api.icraft21.com/prod/delete',
  data: JSON.stringify({
    "object_id": 1,
    "object_name": "news",
  }),
  contentType: 'application/json',
  success: completeRequest,
  error: errorRequest
});
```

> Example Response:

```json
{
	"statusCode": 200
}
```

공시 정보 삭제. `DELETE` 메소드는 등록된 사용자만 `access token`을 가지고 사용 할수 있으며, 위 `Authentication` 내용을 참고 부탁드립니다.  

### Query String

Parameter | Type | Description
--------- |----------- |-----------
object_id | int | 아이디 값
object_name | string | object 이름


# Recruit

## GET

> Example Request

```javascript
$.ajax({
  method: 'GET',
  url: 'https://api.icraft21.com/prod/recruit?page=' + pageNum + '&search' + searchString,
  contentType: 'application/json',
  success: completeRequest,
  error: errorRequest
});
```

> Example Response:

```json
{
  "statusCode": 200,
  "body": {
    "result": [
      {
        "id": 14,
        "title": "아이크래프트 2018년도 상반기 신입사원 모집공고",
        "date": "2018-01-05",
        "kinds": "신입",
        "available": true,
        "count": "221"
      },
      {
        "id": 13,
        "title": "아이크래프트 2017년도 하반기 경력사원 모집공고",
        "date": "2017-06-29",
        "kinds": "경력",
        "available": false,
        "count": "584"
      }
    },
    "total": 14
  }
}
```

Recruit는 채용 게시판 정보입니다.

### Query String

Parameter | Type | Description
---------|-----------|-----------
page | optional, default=1 | 페이지 수, 10개 단위로 페이지가 구분됩니다.
search | optional | 검색어와 `title`을 비교하여, 검색어가 포함된 경우 결괏값을 전달합니다.
recruit_id | optional | 개별 `object`에 관한 세부 정보를 보고 싶을 경우, `id`를 전송하면 기사 세부 내용을 전달합니다.

### Returns

Parameter | Description
--------- |-----------
id | 번호
title | 제목
date | 시간
kind | 채용 구분
available | 상태
count | 조회수
total | 전체 `Recruit object` 갯수
body | 내용 (`recruit_id`로 호출할 경우만 리턴)

## POST

> Example Request

```javascript
$.ajax({
  method: 'POST',
  url: 'https://api.icraft21.com/prod/recruit,
  data: JSON.stringify({
    "title": title,
    "body": body,
    "kinds": "경력",
    "date": "2018-06-29",
    "available": false
  }),
  contentType: 'application/json',
  success: completeRequest,
  error: errorRequest
});
```

> Example Response:

```json
{
  "statusCode": 201,
}
```

아이크래프트 채용 정보 등록. `POST` 메소드는 등록된 사용자만 `access token`을 가지고 사용 할수 있으며, 위 `Authentication` 내용을 참고 부탁드립니다. 

### Query String

Parameter | Type | Description
--------- |----------- |-----------
title | string | 제목
body | html | 내용
kinds | string | 지원 구분
date | optional, default=now(), date | 날짜, 형식: '%Y-%m-%d'
available | optional, default=false, bool | 지원 가능 여부
recrtui_id | optional, default=-1 | 기본값으로 보내거나 포함시키지 않을 경우 자동으로 신규 object가 생성됩니다. id가 보내진다면 해당 내용으로 업데이트합니다.


## DELETE

> Example Request

```javascript
$.ajax({
  method: 'POST',
  url: 'https://api.icraft21.com/prod/delete',
  data: JSON.stringify({
    "object_id": 1,
    "object_name": "recruit",
  }),
  contentType: 'application/json',
  success: completeRequest,
  error: errorRequest
});
```

> Example Response:

```json
{
	"statusCode": 200
}
```

공시 정보 삭제. `DELETE` 메소드는 등록된 사용자만 `access token`을 가지고 사용 할수 있으며, 위 `Authentication` 내용을 참고 부탁드립니다.  

### Query String

Parameter | Type | Description
--------- |----------- |-----------
object_id | int | 아이디 값
object_name | string | object 이름


# Contact

## POST

> Example Request

```javascript
$.ajax({
  method: 'POST',
  url: 'https://api.icraft21.com/prod/contact,
  data: JSON.stringify({
    "title": title,
    "name": name,
    "email": email,
    "message": message,
  }),
  contentType: 'application/json',
  success: completeRequest,
  error: errorRequest
});
```

> Example Response:

```json
{
  "statusCode": 201,
}
```

아이크래프트 채용 정보 등록. `POST` 메소드는 등록된 사용자만 `access token`을 가지고 사용 할수 있으며, 위 `Authentication` 내용을 참고 부탁드립니다. 

### Query String

Parameter | Type | Description
--------- |----------- |-----------
title | string | 제목
name | string | 이름
email | email | 이메일
message | string | 문의사항