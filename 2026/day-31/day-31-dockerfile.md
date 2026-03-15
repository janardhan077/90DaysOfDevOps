1️⃣ Image with CMD
Dockerfile
FROM ubuntu:latest
CMD ["echo", "hello"]
Build
docker build -t cmd-test .
Run container
docker run cmd-test

Output

hello
Run with custom command
docker run cmd-test date

Output

(current date and time)
What happens

The CMD gets replaced by the new command.

Docker runs date instead of echo hello.

2️⃣ Image with ENTRYPOINT
Dockerfile
FROM ubuntu:latest
ENTRYPOINT ["echo"]
Build
docker build -t entry-test .
Run container
docker run entry-test hello

Output

hello
Run with more arguments
docker run entry-test hello world

Output

hello world
What happens

The ENTRYPOINT stays fixed.

Any extra arguments are added to the command.

Docker actually runs:

echo hello world
3️⃣ When to use
CMD

Use CMD when:

You want a default command

Users can easily replace it

Example: running scripts or temporary commands.

ENTRYPOINT

Use ENTRYPOINT when:

The container should always run a specific program

Users only pass arguments

Example: CLI tools or fixed applications.

Simple difference
CMD	ENTRYPOINT
Default command	Fixed command
Can be overridden	Cannot be replaced easily
Flexible	More strict
