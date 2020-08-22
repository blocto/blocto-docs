---
description: You can send reward to your users as a way of incentive
---

# Rewards

### Domains

| Environment | Domain |
| :--- | :--- |
| Staging | api-staging.blocto.app |
| Production | api.blocto.app |

### Header

```http
Authorization: <JWT>
Content-Type: application/json
```

### APIs

{% api-method method="get" host="https://api.blocto.app" path="/reward/send" %}
{% api-method-summary %}
Send reward
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="blockchain" type="string" required=true %}
dApp blockchain
{% endapi-method-parameter %}

{% api-method-parameter name="wallet\_address" type="string" required=true %}
user's wallet address on the blockchain 
{% endapi-method-parameter %}

{% api-method-parameter name="point" type="integer" required=true %}
the amount of Blocto points to send
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  'message_code': ok
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

