#Building
docker build -t manticore-sdl-core:master --build-arg CORE_VERSION=master .




docker run -d --name manticore-sdl-core-develop manticore-sdl-core:develop
