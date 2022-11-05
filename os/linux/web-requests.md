# web-requests

## CURL

### SSL Certificate

#### Verify SSL certificate of a domain

```
curl https://maheshrjl.com -vI
```

#### Check expiry date

```
curl https://maheshrjl.com -vI --stderr - | grep "expire date"
```
