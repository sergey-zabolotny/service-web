#!/usr/bin/env bash
set -e

if [[ -n "${DEBUG}" ]]; then
    set -x
fi

_gotpl() {
    if [[ -f "/etc/gotpl/$1" ]]; then
        gotpl "/etc/gotpl/$1" > "$2"
    fi
}

process_templates() {
    _gotpl "nginx.conf.tmpl" "/etc/nginx/nginx.conf"
    _gotpl "ssl.conf.tmpl" "/etc/nginx/ssl.conf"
    _gotpl "vhost.conf.tmpl" "/etc/nginx/conf.d/vhost.conf"

    _gotpl "includes/defaults.conf.tmpl" "/etc/nginx/defaults.conf"
    _gotpl "includes/fastcgi.conf.tmpl" "/etc/nginx/fastcgi.conf"

    if [[ -n "${NGINX_VHOST_PRESET}" ]]; then
        _gotpl "presets/${NGINX_VHOST_PRESET}.conf.tmpl" "/etc/nginx/preset.conf"

        if [[ "${NGINX_VHOST_PRESET}" =~ ^drupal8|drupal7|drupal6|wordpress|php$ ]]; then
            _gotpl "includes/upstream.php.conf.tmpl" "/etc/nginx/upstream.conf"
        elif [[ "${NGINX_VHOST_PRESET}" == "http-proxy" ]]; then
            _gotpl "includes/upstream.http-proxy.conf.tmpl" "/etc/nginx/upstream.conf"
        fi
    fi

    _gotpl "50x.html.tmpl" "/usr/share/nginx/html/50x.html"
}

process_templates

# Basic HTTP Authentication
if [[ "$APACHE_BASIC_AUTH_USER" != "" ]] && [[ "$APACHE_BASIC_AUTH_PASS" != "" ]]; then
	echo "Enabling Basic HTTP Authentication [$APACHE_BASIC_AUTH_USER:$APACHE_BASIC_AUTH_PASS]"
	echo "$APACHE_BASIC_AUTH_USER:$(echo $APACHE_BASIC_AUTH_PASS | mkpasswd -m md5)" >/etc/nginx/htpasswd
	echo 'auth_basic "Restricted area";' >/etc/nginx/conf.d/basic-auth.conf
	echo 'auth_basic_user_file htpasswd;' >>/etc/nginx/conf.d/basic-auth.conf
fi

gen_ssl_certs /etc/nginx/ssl
exec nginx
