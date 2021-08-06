# AmazonAd

- 获取授权码
> 授权码用于生产授权和刷新令牌
- 参数
    - client_id
    - scope 
    > DSP, Sponsored Brands, Sponsored Display, Sponsored Products, and Amazon Attribution APIs 时设置为 `advertising::campaign_management`
    - response_type 默认值 `code`
    - redirect_uri 重定向url
```
https://www.amazon.com/ap/oa?client_id=YOUR_LWA_CLIENT_ID&scope=advertising::campaign_management&response_type=code&redirect_uri=YOUR_RETURN_URL
```
- 登陆
 > 点击授权按钮之后登陆
- 授权
- 携带`code`重定向到redirect_uri
- 调用 authorization_url 获取 `refresh_token` 
    - 参数 
        - grant_code 固定值`authorization_code`
        - code 
        - redirect_uri
        - client_id
        - client_secret
    - headers
        - Content-Type:application/x-www-form-urlencoded
        - charset=UTF-8

## Profiles(配置文件)
### GetProfiles (获取配置信息)
**url**
- /v2/profiles

**response**
```json
[
  {
    "profileId": 0, //后续请求需要携带profieId
    "countryCode": "US",
    "currencyCode": "USD",
    "dailyBudget": 0,
    "timezone": "America/Los_Angeles",
    "accountInfo": {
      "marketplaceStringId": "string",
      "id": "string",
      "type": "vendor",
      "name": "string",
      "subType": "KDP_AUTHOR",
      "validPaymentMethod": true
    }
  }
]
```

### 更新每日预算
**URL**
- /v2/profiles [POST]
**请求**
```json
[
  {
    "profileId": 0,
    "dailyBudget": 0,
    "accountInfo": {}
  }
]
```

## 广告组合


## 活动
### 创建活动
**URL**
- /v2/sp/campaingns

**请求参数**
```json
[
  {
    "portfolioId": 0, 
    "name": "string", //活动名称
    "tags": {
      "PONumber": "examplePONumber",
      "accountManager": "exampleAccountManager"
    },
    "campaignType": "sponsoredProducts", //活动类型 
    "targetingType": "manual", //活动目标类型
    "state": "enabled", //状态
    "dailyBudget": 0, //每日预算
    "startDate": "string", //开始时间
    "endDate": "string", //结束时间
    "premiumBidAdjustment": true,
    "bidding": {
      "strategy": "legacyForSales", //投标策略 legacyForSales(动态出价仅向下), autoForSales(动态出价), manual(固定出价)
      "adjustments": [
        {
          "predicate": "placementTop", //根据放置位置来调整出价 placementTop , placementProductPage
          "percentage": 0 //投标调整百分比 
        }
      ]
    }
  }
]
```

### 获取活动列表
**URL**
- /v2/sp/campaigns

### 获取指定活动
**URL**
- /v2/sp/campaigns/{campaignId}

### 归档活动
> 归档状态下午无法再次激活
**URL**
- /v2/sp/campaigns/{campaignId}

### 获取具有扩展字段的活动列表
**URL**
- /v2/sp/campaigns/extended

```json
[
  {
    "portfolioId": 0,
    "campaignId": 0,
    "tags": {
      "PONumber": "examplePONumber",
      "accountManager": "exampleAccountManager"
    },
    "name": "string",
    "campaignType": "sponsoredProducts",
    "targetingType": "manual",
    "state": "enabled",
    "dailyBudget": 0,
    "startDate": "string",
    "endDate": "string",
    "premiumBidAdjustment": true,
    "bidding": {
      "strategy": "legacyForSales",
      "adjustments": [
        {
          "predicate": "placementTop",
          "percentage": 0
        }
      ]
    },
    "placement": "placementTop", //广告投放类型
    "creationDate": 0, //活动的创建日期
    "lastUpdatedDate": 0, //上次修改日期
    "servingStatus": "CAMPAIGN_ARCHIVED" //活动的计算状态
  }
]
```

## 广告分组
### 创建广告组
**URL**
- /v2/sp/adGroups [POST]

**请求**
```json
[
  {
    "name": "string", //广告组名称
    "campaignId": 0, //与广告组关联的现有活动
    "defaultBid": 0, // 广告组中没有为关键字指定出价时使用的出价值  最小0.02
    "state": "enabled" //当前状态 enabled pause archived
  }
]
```

### 更新广告组
**URL**
- /v2/sp/addGroups [PUT]

### 获取广告组列表
**URL**
- /v2/sp/addGroups [GET]

### 获取指定广告组
**URL**
- /v2/sp/adGroups/{adGroupId} [GET]

### 规定广告组
**URL**
- /v2/sp/adGroups/{adGroupId} [DELETE]

### 获取带有扩展字段的广告组
**URL**
- /v2/sp/adGroups/extended [GET]
**响应**
```json
[
  {
    "adGroupId": 0,
    "name": "string",
    "campaignId": 0,
    "defaultBid": 0,
    "state": "enabled",
    "creationDate": 0, //创建时间
    "lastUpdatedDate": 0, //上次更新时间
    "servingStatus": "AD_GROUP_ARCHIVED" //服务状态
  }
]
```

## 出价建议
### 获取广告组的出价建议
**URL**
- /v2/sp/adGroups/{adGroupId}/bidRecommendations [GET]
**响应**
```json
{
  "adGroupId": 0, //广告组id
  "suggestedBid": {
    "suggested": 0, //建议出价
    "rangeStart": 0, //下限
    "rangeEnd": 0 //上限
  }
}
```

### 获取关键词的出价建议
**URL**
- /v2/sp/keywords/{keywordId}/bidRecommendations [GET]
**响应**
```json
{
  "keywordId": 0, //关键词id
  "adGroupId": 0,
  "suggestedBid": {
    "suggested": 0,
    "rangeStart": 0,
    "rangeEnd": 0
  }
}
```

### 获取关键词的出价建议
**URL**
- /v2/sp/keywords/bidRecommendations [POST]
**请求**
```json
{
  "adGroupId": 0, //广告组id
  "keywords": [
    {
      "keyword": "string", //关键词
      "matchType": "exact" //匹配类型 excat(精准) phrase(短语) broad(宽泛)
    }
  ]
}
```
**响应**
```json
{
  "adGroupId": "string",
  "recommendations": [
    {
      "code": "SUCCESS",
      "keyword": "string",
      "matchType": "exact",
      "suggestedBid": {
        "suggested": 0,
        "rangeStart": 0,
        "rangeEnd": 0
      }
    }
  ]
}
```

### 获取关键字、产品或自动目标表达式的投标建议列表
**URL**
- /v2/sp/targets/bidRecommendations [POST]
**请求**
```json
{
  "adGroupId": 0,
  "expressions": [ //目标表达式列表
    {
      "value": "string",  //值
      "type": "queryBroadMatches" //类型
    }
  ]
}
```

## 关键词
### 获取指定关键词
**URL**
- /v2/sp/keywords/{keywordId} [GET]
**响应**
```json
{
  "keywordId": 0, //关键词id
  "campaignId": 0, //与关键词关联的活动id
  "adGroupId": 0, //关键词关联的广告组id
  "state": "enabled", //状态
  "keywordText": "string", //关键词
  "nativeLanguageKeyword": "string", // 未本地话的关键词文本
  "matchType": "exact", //匹配类型
  "bid": 0 //关键词关键的出价
}
```

### 归档关键词
**URL**
- /v2/sp/keywords/{keywordId} [DELETE]

### 获取带有扩展字段的关键词
**URL**
- /v2/sp/keywords/extended/{keywordId} [GET]

**响应**
```json
{
  "keywordId": 0,
  "campaignId": 0,
  "adGroupId": 0,
  "state": "enabled",
  "keywordText": "string",
  "nativeLanguageKeyword": "string",
  "matchType": "exact",
  "bid": 0,
  "creationDate": 0,
  "lastUpdatedDate": 0,
  "servingStatus": "TARGETING_CLAUSE_ARCHIVED" //服务状态
}   
```

### 获取关键词列表
**URL**
- /v2/sp/keywords [GET]
**响应**
```json
[
  {
    "keywordId": 0,
    "campaignId": 0,
    "adGroupId": 0,
    "state": "enabled",
    "keywordText": "string",
    "nativeLanguageKeyword": "string",
    "matchType": "exact",
    "bid": 0
  }
]
```

### 创建关键词
**URL**
- /v2/sp/keywords [GET]
**请求**
```json
[
  {
    "campaignId": 0,
    "adGroupId": 0,
    "state": "enabled",
    "keywordText": "string",
    "nativeLanguageKeyword": "string",
    "nativeLanguageLocale": "string",
    "matchType": "exact",
    "bid": 0 //最小值0.02
  }
]
```

### 更新关键词
**URL**
- /v2/sp/keywords [PUT]

## 建议关键词 
### 获取指定广告组的建议关键词
**URL**
- /v2/sp/adGroups/{adGroupId}/suggested/keywords [GET]

**响应**
```json
{
  "adGroupId": 0,
  "suggestededKeywords": [ //建议关键词列表
    "string"
  ]
}
```

### 获取指定广告带有扩展数据的建议关键词
**URL**
- /v2/sp/adGroups/{adGroupId}/suggested/keywords/extended [GET]
```json
[
  {
    "adGroupId": 0, //广告组id
    "campaignId": 0, //活动id
    "keywordText": "string", //建议关键词
    "matchType": "exact", 
    "state": "enabled",
    "bid": 0
  }
]
```

### 获取指定ASIN 的建议关键词
**URL**
- /v2/sp/asins/{asinValue}/suggested/keywords [GET]
```json
{
  "asin": "string",
  "suggestedKeywords": [
    {
      "keywordText": "string",
      "matchType": "exact"
    }
  ]
}
```

### 产品广告
### 获取指定产品广告
- /v2/sp/productAds/{adId} [GET]
**响应**
```json
{
  "adId": 0,
  "campaignId": 0,
  "adGroupId": 0,
  "sku": "string",
  "asin": "string",
  "state": "enabled"
}
```

### 归档产品广告
- /v2/sp/productAds/{adId} [DELETE]

### 获取指定条件的产品广告列表
- /v2/sp/productAds [GET]

### 创建产品广告
- /v2/sp/productAds [POST]

### 更新产品广告
- /v2/sp/productAds [PUT]



