SoX: Конвертация файлов для Asterisk 
Конвертация файлов для Asterisk при помощи программы SoX

[[sox|sox]] — консольная утилита, которая умеет конвертировать аудиофайлы. 

## узнать информацию об аудиофайле 
```
sox -V file.wav -e stat
```

## wav to gsm 
```
sox start.wav -r 8000 -c 1 -s -w finish.gsm resample -ql
```

## gsm to wav 
```
sox start.gsm -r 44100 -a finish.wav
```
или
```
sox start.gsm -t wav -e signed-integer finish.wav
```
## wav to alaw 
```
sox start.wav -A -t RAW -r 8000 -c 1 finish.alaw
```

## alaw to wav 
```
sox -A -t RAW -r 8000 -c 1 start.alaw finish.wav
```

# Команды для конвертации всех файлов в каталоге (применяется в скриптах) 

## Wav to alaw 

```
for i in *.wav; do sox ./$i -t RAW -A -r 8000 -c 1 -1 ./`echo $i| sed "s/wav/alaw/"`; done
```

## Wav to ulaw 

```
for a in *.wav; do sox "$a" -t raw -r 8000 -c 1 -b -U `echo $a|sed "s/.wav/.mulaw/"` ; done
```
## Wav to alaw 

```
for a in *.wav; do sox "$a" -t raw -r 8000 -c 1 -b -A `echo $a|sed "s/.wav/.alaw/"` ; done
```

## Wav to gsm 

```
for a in *.wav; do sox "$a" -r 8000 -c1 `echo $a|sed "s/.wav/.gsm/"` resample -ql; done
```

## Alaw to wav 

```
for a in *.alaw; do sox -A -t RAW -r 8000 -c 1 "$a" `echo $a|sed "s/.alaw/.wav/"` ; done
```

# Работа с MP3

Для работы с MP3 необходимо загрузить дополнительно 2 библиотеки: liblame и libmad
Под Windows эти библиотеки можно загрузить [[http://ossbuild.googlecode.com/svn/trunk/Shared/Build/Windows/Win32/bin/|отсюда]]: 
[[http://ossbuild.googlecode.com/svn/trunk/Shared/Build/Windows/Win32/bin/libmad-0.dll|libmad-0.dll]]
[[http://ossbuild.googlecode.com/svn/trunk/Shared/Build/Windows/Win32/bin/libmp3lame-0.dll|libmp3lame-0.dll]]
Эти файлы необходимо положить в папку с sox.exe

## MP3 в GSM 
```
sox source.mp3 -r 8k -c 1 -e gsm-full-rate finish.gsm remix -
```

## MP3 в WAV 

```
sox source.mp3 -c 1 -t wav -r 8k finish.wav remix -
```

# Воспроизведение
В папке с sox.exe необходимо выполнить команду
```
copy sox.exe play.exe
```
Затем использовать play.exe в качестве консольного проигрывателя


