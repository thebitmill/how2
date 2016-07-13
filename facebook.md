# Facebook SDK

## Get Access Token from Signed Request Code

```
const full = url.format({
  protocol: 'https',
  host:  'graph.facebook.com/oauth/access_token',
  query: {
    client_id: facebook.id,
    client_secret: facebook.secret,
    redirect_uri: '',
    code: signed.code
  }
});

superagent.get(full)
  .set('Accept', 'application/json')
  .end(function (err, response) {
    console.log('signed request');
    const accessToken = response.text.split('&')[0].split('=')[1];
    console.log(accessToken);
  });
```
