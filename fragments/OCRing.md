First install the necessaries

```sh
sudo apt-get install tesseract-ocr tesseract-ocr-eng xpdf imagemagick xpdf-utils
```

Create script called scripts/pdfocrtotext.sh

```sh
#!/bin/sh
mkdir tmp
cp $@ tmp
cd tmp
pdftoppm * -f 1 -l 10 -r 600 ocrbook
for i in *.ppm; do convert "$i" "`basename "$i" .ppm`.tif"; done
for i in *.tif; do tesseract "$i" "`basename "$i" .tif`" -l eng; done
for i in *.txt; do cat $i >> ${2}.txt; echo "[pagebreak]" >> ${2}_pdf-ocr-output.txt; done
mv ${2}_pdf-ocr-output.txt ..
rm -rf *
cd ..
rmdir tmp
```

Run like this pdfocrtotext.sh inputfilename outputfilename


pdfocrtotext.sh inputfilename outputfilename
