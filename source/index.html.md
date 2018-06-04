---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python

toc_footers:
  - <a href='https://blackruby.studio'>BlackRuby Homepage</a>

includes:
  - errors

search: true
---

# Introduction

## iMMS API

iMMS API는 iMMS 서버에 연동하여 서비스를 제공하기 위한 API이며, 개발자, 영업 사원, 개발 팀장이 자신의 혹의 자신이 소속된 팀의 일정을 볼 수 있는 서비스입니다.

좌측에는 메소드와 파라미터에 대한 설명을 명시했으며, 우측에는 shell과 python을 이용한 코드 예시를 보여줍니다. 우측 상단에서 예시 언어를 변경 할 수 있습니다.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```python
# With shell, you can just pass the correct header with each request

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

```python
curl "http://example.com/api/daily/1/1803
```

> 개발자의 Response:

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

사용자의 이번 달 일정을 호출. header의 토큰값을 기준으로 사용자의  계정과 권한을 확인하여 일정 등록 여부를 결정. 개발자 팀원의 경우, 자신의 일정만 볼 수 있고, 영업직은 자신이 프로젝트 일정을 열람 가능하고, 개발 팀장의 경우 팀 소속 모든 팀원들의 일정 열람 가능.

### HTTP Request

기본 경로: `GET /calendars/`

날짜 명시: `GET /calendars/{yymm}`

팀내 타 사용자의 일정 열람: `GET /calendar/{yymm}/{user}`

### Arguments

Parameter | Description
--------- |-----------
yymm | 날짜
user | 사용자 ID

- 기본 경로로 호출할 경우 로그인한 유저의 이번달 일정 반환. 
- 날짜를 명시해서 호출할 경우 해당 개월의 일정이 반환되며, url은 바뀌지 않게 할 에정. 
- 팀장이 자신의 팀 소속 다른 팀원의 일정에 접속할 경우, `yymm`과 해당 `user`의 ID로 호출. 

### Returns

Parameter | Description
--------- |-----------
user | 사용자 ID
yymm | 날짜
worktimes | 일정 리스트
date | 날짜
time | 근무 시간
project | 진행한 프로젝트 갯수
worker | 프로젝트에 참여한 인원수


## Post my schedule

```shell
curl -X POST "http://example.com/api/daily/2 \
-d user=123 \
-d	date=20180301 \
-d time=8
```

```python
curl -X POST "http://example.com/api/daily/2 \

```

> Response:

```json
{
	"response": 201
}
```

개인별 일정 등록. 개발자의 경우 일정한 규칙에 따라 일정을 등록해야 하며, 규칙에 맞지 않는다면 실패 메세지 반환. 개발 팀장의 경우 근로기준법을 위반하는 입력 값을 제외하고 모두 입력 가능.

### HTTP Request

`POST /calendar/{user}`

### Arguments

Parameter | Description
--------- |-----------
user | 사용자 ID
date | 입력하는 날짜
time | 근무 시간

### Returns

Parameter | Description
--------- |-----------
response | 결괏값

# 통계

## 엑셀 추출

```shell
curl "http://example.com/api/excel/2
```

> Response:

```json
.csv file
```

유형별 엑셀 추출.

### HTTP Request

`GET /excel/{type}`

### Arguments

Parameter | Description
--------- |-----------
type | 통계 유형

### Returns

.csv file
