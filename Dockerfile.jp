FROM alpine:3.11

ARG GLIBC_VERSION="2.30-r0"
ENV LANG=ja_JP.UTF-8

RUN set -x && \
  apk add --no-cache --virtual .build-dependencies ca-certificates wget && \
  wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && \
  wget \
    https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-${GLIBC_VERSION}.apk \
    https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-bin-${GLIBC_VERSION}.apk \
    https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-i18n-${GLIBC_VERSION}.apk && \
  apk add --no-cache \
    glibc-${GLIBC_VERSION}.apk  \
    glibc-bin-${GLIBC_VERSION}.apk \
    glibc-i18n-${GLIBC_VERSION}.apk && \
  mv /usr/glibc-compat/lib/ld-linux-x86-64.so.2 /usr/glibc-compat/lib/ld-linux-x86-64.so.${GLIBC_VERSION} && \
  ln -s /usr/glibc-compat/lib/ld-linux-x86-64.so.${GLIBC_VERSION} /usr/glibc-compat/lib/ld-linux-x86-64.so.2 && \
  ( /usr/glibc-compat/bin/localedef -i /usr/glibc-compat/share/i18n/locales/ja_JP -c -f UTF-8 "ja_JP.UTF-8" || true ) && \
  ( /usr/glibc-compat/bin/localedef -i /usr/glibc-compat/share/i18n/locales/POSIX -c -f UTF-8 "C.UTF-8" || true ) && \
  echo "export LANG=$LANG" > /etc/profile.d/locale.sh && \
  apk del glibc-i18n .build-dependencies && \
  rm -rf glibc-*.apk /etc/apk/keys/sgerrand.rsa.pub /root/.[acpw]* 

CMD ["/bin/sh"]
