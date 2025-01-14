<img align="right" src="https://raw.githubusercontent.com/erdilS/Port-Windows-11-Xiaomi-Pad-5/main/nabu.png" width="425" alt="Windows 11 Running On A Xiaomi Pad 5">


# Installare Windows su mi pad 5

## Aggiornare i driver

### Prerequisiti


- [UEFI image](https://raw.githubusercontent.com/erdilS/Port-Windows-11-Xiaomi-Pad-5/main/images/xiaomi-nabu_secureboot-v2.img)
- [Recovery](../../../../releases/tag/1.0)
- [Drivers](https://github.com/map220v/MiPad5-Drivers/releases/latest)

#### Avvia la recovery tramite PC con il seguente comando: 

```cmd
fastboot boot <recovery.img>
```


#### Esegui lo script

```cmd
adb shell msc
```

### Assegna le lettere ai dischi

#### Avvia il gestore delle partizioni di Windows (diskpart)

> Quando il dispositivo é rilevato come disco

```cmd
diskpart
```


### Assegna la lettera `X` al volume di Windows

#### Seleziona il volume di Windows (WINNABU)
> Usa il comando `list volume` per trovarlo, é quello che si chiama "WINNABU"

```diskpart
select volume <number>
```

#### Assegna la lettera x
```diskpart
assign letter=x
```

#### Esci da diskpart:
```diskpart
exit
```

### Installazione drivers

> È possibile scaricare i driver [qui](https://github.com/map220v/MiPad5-Drivers/releases/latest)

```cmd
Apri la cartella con i driver ed esegui OfflineUpdater.cmd
```


### Avvia Windows con l'immagine UEFI

```
fastboot flash boot <uefi.img>
```

## Finito!
