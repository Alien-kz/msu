all: bin bin/server bin/client
	
bin:
	mkdir bin

bin/server: src/server.c
	gcc src/server.c -o bin/server -fsanitize=leak,address

bin/client: src/client.c
	gcc src/client.c -o bin/client -fsanitize=leak,address

clean:
	rm bin/server bin/client
	rmdir bin

