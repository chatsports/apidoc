# API Documentation

Allows you to add inline documentation to a Sinatra app and have a web-based
documentation browser be available to you.

Staging URL:
  http://chatsportslabs.com:4567/

Production URL:
  http://api.chatsports.com

Example request(this does not use your api key, it uses another one of ours):

http://chatsportslabs.com:4567/sites/search/giant?key=YOUR_API_KEY&signature=67f2d4679abd856c40fa8dd5554a8bb0

## Sites

### url
  ```ruby
    /sites/search/:search_name/
  ```
 
### response
  ```json
    {"teams": [
      {
        "id":   "1234",
        "name": "The Team Name",
        "city": "The City Name",
        "league": "The League Name"
      }
    ] }
  ```

## News IDs
### url
```ruby
  /ids/news/:site_id/:start_time/:end_time
```
### response
```json
    {
      "news_ids":     "[1,2,3,4]",
      "last_updated": "1370405924"
    }
```

## News keyword search
### url
```ruby
  /sites/:site_id/news/keyword_search/:keywords/:start_time/:end_time
```
**note: keywords are comma seperated**
### response
```json
    {"news": [
      {
        "id":      "1234",
        "team_id": "1",
        "title":   "A News Title",
        "content": "Some news article content...",
        "image":   "http://www.example.com/img.jpg",
        "link":    "http://www.example.com/article/1",
        "source_title": "Some news source",
        "source_subtitle": "Has a lot of news",
        "date_created": "1370405924"
      }
    ] }
```

## Single news element
### url
```ruby
  /news/:news_id
```
### response
```json
    {
      "id":      "1234",
      "team_id": "1",
      "title":   "A News Title",
      "content": "Some news article content...",
      "image":   "http://www.example.com/img.jpg",
      "link":    "http://www.example.com/article/1",
      "source_title": "Some news source",
      "source_subtitle": "Has a lot of news",
      "date_created": "1370405924"
    }
```
## Multiple news elements
### url
```ruby
  /sites/:site_id/news/:start_time}/:end_time
```
### response
```json
    {"news": [
      {
        "id":      "1234",
        "team_id": "1",
        "title":   "A News Title",
        "content": "Some news article content...",
        "image":   "http://www.example.com/img.jpg",
        "link":    "http://www.example.com/article/1",
        "source_title": "Some news source",
        "source_subtitle": "Has a lot of news",
        "date_created": "1370405924"
      }
    ]}
```
----
## Signing requests

### given a params hash of
```json
  {
    foo:        "bar",
    baz:        "qux",
    signature:  "dix038942daf",
    key:        "YOUR_API_KEY",
  }
```
```ruby
  def validate_request
    signature = params.delete('signature')
    request_key = params[:key]
    valid = false
    if signature.present? && request_key.present?      
      concats = ""
      params.each do |key, value|
        concats << "#{key}#{value}" if key.present? && value.present?
      end
      
      #this concats your SECRET KEY on the end after the params
      concats << CSAPI.secret_from_key(params[:key])  
      valid = !signature.nil? and Digest::MD5.hexdigest(concats) == signature
    end
    valid
  end
```

Please do testing on using the staging URL.
