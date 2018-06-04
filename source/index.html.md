---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://blackruby.studio'>BlackRuby Homepage</a>

includes:
  - errors

search: true
---

# Introduction

## iMMS API

iMMS API는 iMMS 서버에 연동하여 서비스를 제공하기 위한 API입니다. 
You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/lord/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# 일정

## Get my Calendar

```shell
curl "http://example.com/api/daily/1/1803
```

> 개발자 직원의 Response:

```json
{
	'user': 123,
	'yymm': 1803,
	'worktimes':[
		{
			'date': 20180301,
			'time': 8
		},
		{
			'date': 20180302,
			'time': 6
		}
	]
}
```

> 영업사원이나 팀장의 Response:

```json
{
	'user': 123,
	'yymm': 1803,
	'worktimes': [
		{
			'date': 20180301,
			'project': 3,
			'worker': 8
		},
		{
			'date': 20180302,
			'project': 2,
			'worker': 5
		}
	]
}
```

사용자의 이번 달 일정 호출. 서버에서 header 값의 토큰값을 기준으로 권한을 확인하여 등록 여부 결정. 사용자의 정보를 url에 별도로 명시하여 팀장이 해당 팀 소속 팀원의 일정을 열람 할 수 있음.

### HTTP Request

`GET /daily/`
`GET /daily/{user}`
`GET /daily/{user}/{yymm}`

`user`와 `month`의 경우 모두 생략 가능. `user`가 생략될 경우 자신의 일정을 불러오고, `month`가 생략될 경우 이번달 일정을 불러옴.


## Post my schedule

```shell
curl -X POST "http://example.com/api/daily/2 \
-d user=123 \
-d	date=20180301 \
-d time=8
```

> Response:

```json
{
	"response": 201
}
```

개인별 일정 등록. 개발자 사용자의 경우 일정한 규칙에 따라 일정을 등록해야 하며, 규칙에 맞지 않는다면 실패 메세지 전송. 관리자의 경우 일정 등록에 대해 근로기준법을 위반하는 입력 값외에 모두 입력 가능.

### HTTP Request

`POST /daily/{user}`

### Arguments

Parameter | Description
--------- |-----------
user | 사용자 ID
date | 입력하는 날짜

### Returns

Parameter | Description
--------- |-----------
response | 성공 여부

# 통계

## 엑셀 추출

```shell
curl "http://example.com/api/excel/2
```

> Response:

```json
.csv file
```

데이터 엑셀 추출.

### HTTP Request

`GET /excel/{type}`

### Arguments

Parameter | Description
--------- |-----------
type | 통계 유형

### Returns

.csv file
