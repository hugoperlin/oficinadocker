## Comando para criar uma imagem com o Firefox ##
docker build --no-cache -t firefox .

## Comando para rodar o Firefox ##

docker run --rm --net=host --env="DISPLAY" --volume="$HOME/.Xauthority:/home/developer/.Xauthority:rw" firefox