# Elastic Search Doc
Elastic Search Cheatsheet 7.x

```
curl -X PUT "localhost:9200/aeroscdn?pretty" -H "Content-Type: application/json" -d"
{
  "mappings": {
    "properties": {
      "page_url": {
	"copy_to": "page_url_index"
      },
      "page_url_index": {
        "type": "text",
        "index": true
      }
    }
  }
}
"

curl -X PUT "localhost:9200/aeroscdn/_mapping?pretty" -H "Content-Type: application/json" -d"
{
  "properties": {
    "page_url": {
      "type": "text",
      "index": "true"
    }
  }
}
"
curl -X PUT "localhost:9200/aeroscdn?pretty" -H "Content-Type: application/json" -d"
{
  "mappings": {
    "properties": {
      "page_url": {
	"type": "text",
        "index": true
      }
    }
  }
}
"

curl -X DELETE "localhost:9200/aerosearcherdb/_doc/BSuzJXcBGv9CdLkLJgsq?pretty"


curl -X PUT "localhost:9200/my-index-000001/_doc/1?refresh&pretty" -H "Content-Type: application/json" -d"
{
  "user.id": "kimchy"
}
"

curl -X GET "localhost:9200/aerosearcherdb/_count?pretty" -H "Content-Type: application/json" -d"
{
 "query": {
  "bool": {
    "should": [{
      "multi_match": {
        "query": "cessna skylane",
        "type": "best_fields"
      }
    }],
    "must": [{
      "terms": {
        "status": [
          1,
          11,
          4,
          44
        ]
      }
    }]
  }
 }
}
"


curl -X POST "localhost:9200/aerosearcherdb/_update/NyF7jXYBGv9CdLkLGS4_?pretty" -H "Content-Type: application/json" -d"
{
  "doc": {
    "status": "9"
  }
}
"


curl -X GET "localhost:9200/aerosearcherdb/_search?pretty" -H "Content-Type: application/json" -d"
{
  "size": 0,
  "aggs": {
    "messageField": {
      "filter": {
        "query": {
          "match": {
            "title": "awais"
          }
        }
      }
    },
    "subjectField": {
      "filter": {
        "query": {
          "match": {
            "description": "awais"
          }
        }
      }
    }
  }
}
"



{
    "index": "aerosearcherdb",
    "body": {
        "query": {
            "bool": {
                "should": [
                    {
                        "multi_match": {
                            "query": "cessna skylane",
                            "type": "best_fields",
                            "operator": "and",
                            "tie_breaker": 0.3,
                            "fields": [
                                "title^5",
                                "full_text^4",
                                "photographer^3",
                                "location^3",
                                "description^2",
                                "domain"
                            ]
                        }
                    }
                ],
                "must": [
                    {
                        "match": {
                            "photographer": {
                                "operator": "and",
                                "query": "Joe Statz"
                            }
                        }
                    },
                    {
                        "terms": {
                            "status": [
                                1,
                                11,
                                4,
                                44
                            ]
                        }
                    }
                ],
                "filter": []
            }
        },
        "from": "0",
        "size": "20"
    }
}



curl -X GET "localhost:9200/aerosearcherdb/_search?pretty&explain=true" -H "Content-Type: application/json" -d"
{
  "index": "aerosearcherdb",
  "body": {
    "query": {
      "bool": {
        "should": [{
          "multi_match": {
            "query": "cessna skylane",
            "type": "best_fields",
            "operator": "and",
            "tie_breaker": 0.3,
            "fields": [
              "title^5",
              "photographer^3",
              "location^3",
              "description^2",
              "domain"
            ]
          }
        }],
        "must": [{
          "terms": {
            "status": [
              1,
              11,
              4,
              44
            ]
          }
        }],
        "filter": []
      }
    },
    "from": "0",
    "size": "2"
  }
}
"

curl -X POST "localhost:9200/aerosearcherdb/_update_by_query?pretty" -H "Content-Type: application/json" -d"
{
  "script": {
    "source": "ctx._source.date_added = params.date_added",
    "params": {
      "date_added": "2020-08-01"
    }
  },
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "date_added"
        }
      }
    }
  }
}
"

curl -X POST "localhost:9200/aerosearcherdb/_update_by_query?pretty" -H "Content-Type: application/json" -d"
{
  "script": {
    "source": "ctx._source.date_next_process = params.date_next_process",
    "params": {
      "date_next_process": "2020-10-01 00:00:00"
    }
  },
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "date_next_process"
        }
      }
    }
  }
}
"

curl -X POST "localhost:9200/aerosearcherdb/_update_by_query?pretty" -H "Content-Type: application/json" -d"
{
  "script": {
    "source": "ctx._source.status = params.status",
    "params": {
      "status": 1
    }
  },
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "status"
        }
      }
    }
  }
}
"




curl -X GET "localhost:9200/as-images/_search?pretty" -H "Content-Type: application/json" -d"
{
   "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "date_added"
        }
      }
    }
  },
  "from": "0",
  "size": "2"
}
"

{
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "date_added"
        }
      }
    }
  }
}

curl -X GET "localhost:9200/as-images/_search?pretty" -H "Content-Type: application/json" -d"
{
  "from": "0",
  "size": "2"
}
"

curl -X PUT "localhost:9200/aerosearcherdb?pretty" -H "Content-Type: application/json" -d"
{
  "mappings" : {
    "properties": {
      "user_id": {
        "type": "long"
      }
    }
  }
}
"
curl -X GET "localhost:9200/aerosearcherdb/_search?pretty" -H "Content-Type: application/json" -d"
{
  "size": 0,
  "query": {
      "range": {
          "date_last_updated": {
              "gte": "2021-01-18 00:00:00"
          }
      }
  },
  "aggs": {
    "days": {
      "date_histogram": {
        "field": "date_last_updated",
        "interval": "day"
      },
      "aggs": {
        "domains": {
          "terms": {
            "field": "domain"
          }
        }
      }
    }
  }
}
"

curl -X POST "localhost:9200/aerosearcherdb/_delete_by_query?pretty" -H "Content-Type: application/json" -d'
{
  "query": {
    "term": {
      "domain": "andrewstransport.smugmug.com"
    }
  }
}
'

curl -X POST "localhost:9200/aerosearcherdb/_delete_by_query?conflicts=proceed&pretty" -H "Content-Type: application/json" -d'
{
  "query": {
    "match_phrase": {
      "page_url": "https://www.airteamimages.com/search.php"
    }
  }
}
'

curl -X POST "localhost:9200/aerosearcherdb/_delete_by_query?pretty" -H "Content-Type: application/json" -d"
{
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "domain"
        }
      }
    }
  }
}
"


{
  "range": {
    "date_last_updated": {
      "gte": "2021-01-22 11:00:00",
      "lte": "2021-01-22 11:10:00"
    }
  }
}


4CvtKXcBGv9CdLkLaKWK

3CvtKXcBGv9CdLkLYqXY

curl -X PUT "localhost:9200/aerosearcherdb?pretty" -H "Content-Type: application/json" -d"
{
  "mappings": {
    "added_at": {
      "type": "date",
      "format": "yyyy-MM-dd HH:mm:ss"
    }
  }
}
"

curl -X PUT "localhost:9200/aerosearcherdb?pretty" -H "Content-Type: application/json" -d"
{
  "mappings": {
    "properties": {
      "date_added_es": {
        "type" : "date",
        "format" : "yyyy-MM-dd HH:mm:ss"
      }
    }
  }
}
"

curl -X PUT "localhost:9200/aerosearcherdb/_mapping?pretty" -H "Content-Type: application/json" -d"
{
  "properties": {
    "date_updated_image": {
      "type": "date",
      "format": "yyyy-MM-dd HH:mm:ss"
    }
  }
}
"

curl -X GET "localhost:9200/aerosearcherdb/_explain/0?pretty" -H "Content-Type: application/json" -d"
{
  "query" : {
    "match" : { "message" : "elasticsearch" }
  }
}
"


curl -X GET "localhost:9200/aerosearcherdb/_explain/lNKu23QBAiZ3Tm66AwYM?pretty" -H "Content-Type: application/json" -d"
{
  "query": {
    "bool": {
      "should": [],
      "must": [{
          "multi_match": {
            "query": "airlines",
            "type": "best_fields",
            "minimum_should_match": "50%",
            "fields": [
              "title^5",
              "photographer^3",
              "location^3",
              "description^2",
              "domain"
            ]
          }
        }
      ]
    }
  }
}
"

curl -X GET "localhost:9200/aerosearcherdb/_search?pretty" -H "Content-Type: application/json" -d"
{
  "profile": true,
  "query": {
    "bool": {
      "should": [],
      "must": [{
          "multi_match": {
            "query": "airlines",
            "type": "best_fields",
            "minimum_should_match": "50%",
            "fields": [
              "title^5",
              "photographer^3",
              "location^3",
              "description^2",
              "domain"
            ]
          }
        },
        {
          "terms": {
            "status": [
              1,
              11,
              4,
              44
            ]
          }
        }
      ],
      "filter": []
    }
  },
  "aggs": {
    "domains": {
      "filters": {
        "filters": {
          "full_text": {
            "match": {
              "full_text": "airlines"
            }
          }
        }
      }
    }
  },
  "from": "0",
  "size": "20"
}
"

curl -X GET "localhost:9200/aerosearcherdb/_search?pretty" -H "Content-Type: application/json" -d'
{
  "aggs": {
    "domains": {
      "terms": {
        "field": "domain",
	"size": 10000
      }
    }
  }
}
'


curl -X POST "localhost:9200/aerosearcherdb/_update_by_query?pretty" -H "Content-Type: application/json" -d'
{
  "script": {
    "source": "ctx._source.original_image_url = \"https://www.airteamimages.com/\" + ctx._source.original_image_url"
  },	
  "query": {
    "bool": {
      "must_not": [{
        "match": {
          "original_image_url": "https://www.airteamimages.com"
        }
      }],
      "filter": [{
        "term": {
          "domain": "airteamimages.com"
        }
      }]
    }
  }
}
'


curl -X POST "localhost:9200/aerosearcherdb/_update_by_query?pretty" -H "Content-Type: application/json" -d'
{
  "script": {
    "source": "ctx._source.image_url = ctx._source.image_url.replace(\"_200\", \"_800\")"
  },	
  "query": {
    "bool": {
      "must": [{
        "match": {
          "image_url": "https://www.airteamimages.com"
        }
      }],
      "filter": [{
        "term": {
          "domain": "airteamimages.com"
        }
      }]
    }
  }
}
'

curl -X POST "localhost:9200/aerosearcherdb/_update_by_query?pretty" -H "Content-Type: application/json" -d'
{
  "script": {
    "source": "ctx._source.page_url = \"https://www.airteamimages.com\" + ctx._source.page_url"
  },	
  "query": {
    "bool": {
      "must_not": [{
        "match": {
          "page_url": "https://www.airteamimages.com"
        }
      }],
      "filter": [{
        "term": {
          "domain": "airteamimages.com"
        }
      }]
    }
  }
}
'


```
