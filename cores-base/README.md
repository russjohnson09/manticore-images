#Building
Base handles the most time consuming part which is the initial build of master.

Other builds can use this as a base.



##Image that has all requirements prepped
docker build -t manticore-sdl-core-base .





docker build -t manticore-sdl-core:develop --build-arg CORE_VERSION=develop .


docker run -d --name manticore-sdl-core-develop manticore-sdl-core:develop
