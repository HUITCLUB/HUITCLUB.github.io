---
layout: post
title:  "17.4.1 일자 아치리눅스 버그 'Conflicting files: ca-certificates-utils: /etc/ssl/certs/ca-certificates.crt already exists in filesystem'"
---

영어 읽을 수 있다면 그냥 여기를 보면 된다.

https://www.ostechnix.com/fix-conflicting-files-ca-certificates-utils-etcsslcertsca-certificates-crt-already-exists-filesystem-error-arch-linux/



한글로 보고 싶거나 필요한 것만 보고 싶은 사람들은 여기서 부터 읽으세요.



현재 아치리눅스 시스템을 업그레이드 하면

Conflicting files: ca-certificates-utils: /etc/ssl/certs/ca-certificates.crt already exists in filesystem

라는 말이 뜨면서 업그레이드가 되지 않는다.



간단한 해결 방법

{% highlight ruby linenos %}
def show
  puts "Outputting a very lo-o-o-o-o-o-o-o-o-o-o-o-o-o-o-o-ong lo-o-o-o-o-o-o-o-o-o-o-o-o-o-o-o-ong line"
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}

1. (sudo) pacman -Syuw

w옵션은 download only 이다

의미는 Sync-refresh-sysupgrade-download only



2. (sudo) rm /etc/ssl/certs/ca-certificates.crt
지운다.

3. (sudo) pacman -Su

다운 받아놓은 걸로 설치를 한다
