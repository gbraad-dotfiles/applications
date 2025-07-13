# Analogue clock

## info

  - Source: https://github.com/gbraad-apps/analogue-clock-sciter
  - Website: https://apps.gbraad.nl/analogue-clock/

## vars
```sh
APPNAME="analogue-clock"
```

## install
```sh
mkdir -p ${APPSHOME}/${APPNAME}
curl -sL https://github.com/gbraad-apps/analogue-clock-sciter/releases/download/v0.5/clock-linux.zip -o /tmp/clock.zip
unzip /tmp/clock.zip -d ${APPSHOME}/${APPNAME}
chmod +x ${APPSHOME}/${APPNAME}/clock
rm -f /tmp/clock.zip
```

## check
```sh
[ -x ${APPSHOME}/${APPNAME}/clock ]
```

## alias default run run-desktop
```sh
if apps analogue-clock check; then 
    ${APPSHOME}/${APPNAME}/clock
else
    echo "Not installed"
fi
```

