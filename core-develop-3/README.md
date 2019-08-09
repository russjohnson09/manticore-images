#Building
Base handles the most time consuming part which is the initial build of master.

Other builds can use this as a base.



##Image that has all requirements prepped
docker build -t manticore-sdl-core-develop-2 .







#Building
docker build -t manticore-sdl-core:master --build-arg CORE_VERSION=master .




docker run -d --name manticore-sdl-core-develop manticore-sdl-core:develop
