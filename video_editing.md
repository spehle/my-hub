## Video Editing

1. Fall noch nicht geschehen, mit Hilfe der Cookie-Extension im Browser, eine Cookie-Datei mit den Login-Daten von Beacon erstellen.
2. Die genaue Bezeichnung des Untertitel-Streams herausbekommen

```
yt-dlp --list-subs "URL des Videos"
```

3. Die Dateien mit yt-dlp herunterladen

```
yt-dlp --cookies "Name des Cookiefiles.txt" --write-sub --sub-lang English --convert-subs srt -f bestvideo+bestaudio/best "URL des Videos"
```

4. Die Länge des ersten Videos auslesen

```
ffprobe -i "Name der Videodatei.mp4" -show_entries format=duration -v error -of csv="p=0"
```

5. Die zweite Untertitel-Datei mit Hilfe von [Subshifter](https://subshifter.bitsnbites.eu/) um die Länge des ersten Videos shiften.

6. Die beiden Untertitel-Dateien kombinieren

```
cat 1.srt 2.srt > comp.srt
```

7. Die Videodatei in TS-Dateien konvertieren

```
ffmpeg -i 1.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts 1.ts
```

8. Die beiden Videos kombinieren

```
ffmpeg -i "concat:1.ts|2.ts" -c copy -bsf:a aac_adtstoasc combined_video.mkv
```

9. Die Untertitel-Datei in das kombinierte Video einbetten

```
ffmpeg -i combined_video.mkv -i comp.srt -c copy -c:s srt -metadata:s:s:0 language=eng -metadata:s:s:0 title="English Subtitles" final_output.mkv
```
