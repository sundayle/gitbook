# Ansible 不转义保留双引号
```
- { path: "{{ nginx_source_path }}/nginx-{{nginx_version }}/src/core/nginx.h",regexp: '"NGINX"',replace: '"SUNDAY"' }
```



