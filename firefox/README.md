## Comando para rodar o Firefox ##

docker run --rm --net=host --env="DISPLAY" --volume="$HOME/.Xauthority:/home/developer/.Xauthority:rw" firefox