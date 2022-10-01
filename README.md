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
или
```
sox input.wav -r 8000 -c 1 -t gsm output.gsm
```
## gsm to wav 
```
sox start.gsm -r 44100 -a finish.wav
```
или
```
sox start.gsm -t wav -e signed-integer finish.wav
```
или

```
sox input.gsm -r 8000 -c 1 -w -s ouput.wav
```
## wav to alaw 
```
sox start.wav -A -t RAW -r 8000 -c 1 finish.alaw
```

## alaw to wav 
```
sox -A -t RAW -r 8000 -c 1 start.alaw finish.wav
```

## ffmpeg compatible to wav
```
ffmpeg -i file.m4a -ac 1 -ab 128k -ar 8000 -acodec pcm_s16le file.wav
```
PS: Cписок поддерживаемых декодеров ``` ffmpeg -codecs | grep D[E.]```

## wav to mp3 (lame)
```
lame -V2 input.wav output.mp3
```

## wav to g722
```
ffmpeg -i vm-intro.wav -ar 16000 -acodec g722 vm-intro.g722
```
PS: Описание  http://wiki.innovaphone.com/index.php?title=Howto:Convert_wave_files_in_to_G722_coder_files


## wav to opus
```
ffmpeg -i sound.wav -acodec libopus -ar 8000 -ab 8000 sound.ogg 
```

## opus to wav 
```
opusdec --force-wav sound.ogg sound.wav
```

## wav repair
```
sox -t raw -r 8000 -b 16 -c 1 -L -e signed-integer broken.wav fixed.wav
```

# Команды для конвертации всех файлов в каталоге (применяется в скриптах) 

## wav to alaw 

```bash
for i in *.wav; do sox ./$i -t RAW -A -r 8000 -c 1 -1 ./`echo $i| sed "s/wav/alaw/"`; done
```

## wav to ulaw 

```bash
for a in *.wav; do sox "$a" -t raw -r 8000 -c 1 -b -U `echo $a|sed "s/.wav/.mulaw/"` ; done
```
## wav to alaw 

```bash
for a in *.wav; do sox "$a" -t raw -r 8000 -c 1 -b -A `echo $a|sed "s/.wav/.alaw/"` ; done
```

## wav to gsm 

```bash
for a in *.wav; do sox "$a" -r 8000 -c1 `echo $a|sed "s/.wav/.gsm/"` resample -ql; done
```

## alaw to wav 

```bash
for a in *.alaw; do sox -A -t RAW -r 8000 -c 1 "$a" `echo $a|sed "s/.alaw/.wav/"` ; done
```

## mp3 to wav 
```bash
for f in *.mp3; do
    lame --decode $f - | sox -v 0.5 -t wav - -t wav -b 16 -r 8000 -c 1 $(basename -s .mp3 $f).wav
done
```

## ffmpeg to wav 
```bash
for f in *.*; do 
    ffmpeg -i "$f" -ac 1 -ab 128k -ar 8000 -acodec pcm_s16le "${f%.*}.wav"
done
```

# Работа с mp3

Для работы с mp3 необходимо загрузить дополнительно 2 библиотеки: liblame и libmad
Под Windows эти библиотеки можно загрузить [[http://ossbuild.googlecode.com/svn/trunk/Shared/Build/Windows/Win32/bin/|отсюда]]: 
[[http://ossbuild.googlecode.com/svn/trunk/Shared/Build/Windows/Win32/bin/libmad-0.dll|libmad-0.dll]]
[[http://ossbuild.googlecode.com/svn/trunk/Shared/Build/Windows/Win32/bin/libmp3lame-0.dll|libmp3lame-0.dll]]
Эти файлы необходимо положить в папку с sox.exe

## mp3 в gsm 
```
sox source.mp3 -r 8k -c 1 -e gsm-full-rate finish.gsm remix -
```

## mp3 в wav 

```
sox source.mp3 -c 1 -t wav -r 8k finish.wav remix -
```
## mp3 to wav (lame)
```
lame --decode file.mp3 - | sox -v 0.5 -t wav - -t wav -b 16 -r 8000 -c 1 file.wav
```

## amr to mp3
```
ffmpeg -i input.amr -ar 12000 output.mp3
```

# Воспроизведение
В папке с sox() необходимо выполнить команду
```
copy sox play
```
Затем использовать play в качестве консольного проигрывателя


