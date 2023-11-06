get 이외의 http 메소드로 접근할 시, 전부 get을 호출하는 것을 보면,
이 RedirectView는 무조건 유저에게 redirect url을 제공하고
그 url로 access 시키는 용도로 사용되는 것 같다.
개발자로서는 url의 변경으로 인해 다른 url에 로직을 작성하고
유저가 제대로 바뀐 곳으로 접속할 수 있도록 제어하는 용도로 사용하는 것 같다.

```python

class RedirectView(View):
    """Provider a redirect on any GET request."""
    permanent = False
    url = None
    pattern_name = None
    query_string = False

    def get_redirect_url(self, *args, **kwargs):
        """
        Return the URL Redirect to. Keyword arguments from the URL pattern
        match generating the redirect request are provided as kwargs to this
        method.
        """
        if self.url:
            url = self.url % kwargs # example.com?pk=%%s
        elif self.pattern_name: # user-detail
            url = reverse(self.pattern_name, args=args, kwargs=kwargs)
        else:
            return None

        if args and self.query_string:
            url = "%s?%s" % (url, args)
        return url

    def get(self, request, *args, **kwargs):
        url = self.get_redirect_url(*args, **kwargs)
        # 응답을 어떤 status code로 할지 정한다.
        # 검색엔진에 링크를 업데이트 시키냐 안시키냐.하는 부분까지도.
        if url:
            if self.permanent:
                return HttpResponsePermanentRedirect(url)
            else:
                return HttpResponseRedirect(url)
        else:
            logger.warning(
                'Gone: %s', request.path,
                extra={'status_code': 410, 'request': request}
            )
            return HttpResponseGone()

    def head(self, request, *args, **kwargs):
        return self.get(request, *args, *kwargs)

    def post(self, request, *args, **kwargs):
        return self.get(request, *args, **kwargs)

    def options(self, request, *args, **kwargs):
        return self.get(request, *args, **kwargs)

    def delete(self, request, *args, **kwargs):
        return self.get(request, *args, **kwargs)

    def patch(self, request, *args, **kwargs):
        return self.get(request, *args, **kwargs)



```


Link
[[python % 포맷팅]]