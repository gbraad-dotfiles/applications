# Copyparty

## info

  - https://www.youtube.com/watch?v=15_-hgsX2V0
  - https://github.com//9001/copyparty


## vars
```sh
COPYPARTYIP=::
COPYPARTYPORT=3923
COPYPARTYVOLUME=/media
```

## pip-install
```sh
python3 -m pip install --user -U copyparty --break-system-packages
```

## default run run-service
```sh interactive
python -m copyparty -i ${COPYPARTYIP} -p ${COPYPARTYPORT} -v ${COPYPARTYVOLUME}::rw
```
