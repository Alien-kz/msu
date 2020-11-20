# Гит

git/webserver.git

## Структура
```
README.md
Makefile
source/
  server.c
  client.c
bin/
  server
  client
resource/
  cgi-source/
    hello-world.c
  cgi-bin/
    hello-world
  html/
    index.html
  multimedia/
    logo.png
```
---------------------------------------------

# Пример работы
## Сервер запуск
```
bin/server 8080 2
```
## Сервер запросы (по сокету)
RECIEVE
```
GET index.html HTTP/1.1\n
Host: 127.0.0.1\n
\n
```

SEND
```
HTTP/1.1 200\n
content-type: html/text\n
content-length: 24\n
\n
<html>hello world</html>
```

## Клиент запуск
```
bin/client
```
## Клиент терминал
```
127.0.0.1:8080/index.html
```
## Клиент запросы (по сокету)
SEND:
```
GET index.html HTTP/1.1\n
Host: 127.0.0.1\n
\n
```

RECIEVE:
```
HTTP/1.1 200\n
content-type: html/text\n
content-length: 24\n
\n
<html>hello world</html>
```
---------------------------------------------

# Пункты
1) GET file.txt

Запрос в браузере
```
http://127.0.0.1:8080/index.html
[http    ] :// [localhost] : [8080] / [index.html]
[protocol]     [host     ]   [port] / [resource  ]
```

Код
```
char* get_text(request_path) {
}

char* add_http_header(text) {
}

int send_to_client(http_answer) {
}

int interaction_client(client_socket) {
  while(1) {
    char *request_text = get_request(client_socket);
    char *request_path = get_path(request_text);
    if (request_is_text(request_path))
      text = get_text(request_path);
      http_answer = add_http_header(text);
      send_to_client(http_answer);
  }
}

main() {
  for (i == 0; i < n; i++) {
    client_socket = accept(...);
    // parent[4] <<====>> client
    pid = fork();
    if (pid == 0) {
      interaction_client(client_socket)
      exit(0);
    }
    close(client_socket);
  }
  ...
  while(1) {
    client_socket = accept(...);
    write(client_socket, "busy", 4);
    close(client_socket);
  }
}
```

2) GET binary

```
https://ru.wikipedia.org/wiki/CGI
```
Common Gateway Interface — «общий интерфейс шлюза») — стандарт интерфейса, используемого для связи внешней программы с веб-сервером
```
http://127.0.0.1:8080/cgi-bin/hello-world
[http    ] :// [127.0.0.1] : [8080] / [cgi-bin/hello-world]
[protocol]     [host     ]   [port] / [resource           ]
```


server.c:
```
int run_binary(path, query_string) {
  pid = fork();
  if (pid == 0) {
    // parent[4] son[4] <<====>> client
    dup2(client_socket, 1);
    // parent[4] son[4] son[1] <<====>> client
    close(client_socket);
    // parent[4] son[1] <<====>> client
    exec("cgi-bin/hello-world");
    exit(0);
  }
  waitpid(pid);
  return 0;
}

interaction_client(client_socket) {
  while(1) {
    char *request_text = get_request(client_socket);
    char *request_path = get_path(request_text);
    if (request_is_binary(request_path))
      ...
      run_binary(request_text);
      ...
    else if (request_is_text(request_path))
      ...
      get_text(...);
      ...
    else if (request_is_multimedia(request_path))
      ...
      get_multimedia(...);
      ...
  }
}
```

3) GET binary query string

4) GET multimedia

5) POST text

6) POST multimedia
