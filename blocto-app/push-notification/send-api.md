# Send API

### Domains

| Environment | Domain                 |
| ----------- | ---------------------- |
| Staging     | api-staging.blocto.app |
| Production  | api.blocto.app         |

### Header

```http
Authorization: <JWT>
Content-Type: application/json
```

### APIs

{% swagger baseUrl="https://api.blocto.app" path="/notification/send" method="post" summary="Send push notification" %}
{% swagger-description %}
Send push notification to users by either user IDs or tags.
{% endswagger-description %}

{% swagger-parameter in="body" name="text" type="string" %}
the message shown on the push notification
{% endswagger-parameter %}

{% swagger-parameter in="body" name="url" type="string" %}
the URL to direct users to 
{% endswagger-parameter %}

{% swagger-parameter in="body" name="tags" type="array" %}
user tags to send push notification to
{% endswagger-parameter %}

{% swagger-parameter in="body" name="user_ids" type="array" %}
user IDs to send push notification to
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
  'message_code': ok
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.blocto.app" path="/notification/tag" method="get" summary="Tag users by user ID" %}
{% swagger-description %}
Set tag to a group of users for future push.
{% endswagger-description %}

{% swagger-parameter in="body" name="tag" type="string" %}
user tag
{% endswagger-parameter %}

{% swagger-parameter in="body" name="user_ids" type="array" %}
user IDs to set tag
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
  'message_code': ok
}
```
{% endswagger-response %}
{% endswagger %}

