---
title: elasticsearch同时搜索多个字段
date: 2022-10-18 16:26:32
tags: elasticsearch,multi_match
---

在使用es做全文检索时,可以使用`match`来查询某个字段,如要查询标题中含有`elasticsearch`相关的内容:

```
$client->search(
    [
        'index' => 'test',
        'body' => [
            'query' => [
                'bool' => [
                    'must' => [
                        [
                            'match' => [
                                'title' => [
                                    'query' => 'elasticsearch'
                                ]
                            ]
                        ]
                    ]
                ]
            ]
        ]
    ]
);
```

现在需要检索标题、副标题、内容都含有`elasticsearch`相关的内容就可以使用`multi_match`:

```
$client->search(
    [
        'index' => 'test',
        'body' => [
            'query' => [
                'bool' => [
                    'must' => [
                        [
                            'multi_match' => [
                                'query' => 'elasticsearch',
                                'fields' => [
                                    'title',
                                    'subtitle',
                                    'content'
                                ]
                            ]
                        ]
                    ]
                ]
            ]
        ]
    ]
);
```