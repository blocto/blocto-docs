---
description: You can send reward to your users as a way of incentive
---

# Rewards

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

{% swagger baseUrl="https://api.blocto.app" path="/reward/send" method="get" summary="Send reward" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="blockchain" type="string" %}
dApp blockchain
{% endswagger-parameter %}

{% swagger-parameter in="body" name="wallet_address" type="string" %}
user's wallet address on the blockchain 
{% endswagger-parameter %}

{% swagger-parameter in="body" name="point" type="integer" %}
the amount of Blocto points to send
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
  'message_code': ok
}
```
{% endswagger-response %}
{% endswagger %}

### Demo

You can find the demo project here: [https://github.com/portto/reward-demo](https://github.com/portto/reward-demo)
