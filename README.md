Docker HTTP Node PDF Filler
========================

This image is service used to fill a PDF with a FDF or XFDF file and to
return the resulting PDF using pdftk.

# Deployment using copy of project on repos registry where the image is pre-built

```bash
docker pull eNeRGy2k/PDFtkFiller

docker run -d \
    --name pdf-filler \
    --restart=always \
    -p 127.0.0.1:9021:9021 \
    -e "port=9021" \
    eNeRGy2k/PDFtkFiller
```

# Connecting to the container from the host

```
docker exec -it pdf-filler /bin/bash -c "export TERM=xterm; exec bash"
```

# Installing additional commands to debug with apt-get
If you wanted to install nano or telnet from there for debugging
```
apt-get install telnet
apt-get install nano
```

## Example call from CURL in Bash
This assumes that the docker image was deployed to localhost on port 9021 and that you are in the test directory of this project where there are two files: one named watermark.pdf and another named my.pdf.

```bash
cd test
curl -F "fdffile=@file.xfdf" -F "pdf-to-fill=@pdf-to-fill.pdf" http://localhost:9021/filler > filledpdf.pdf
```

## Example call from CURL in PHP
```php
&lt;?php
  $ch = curl_init("http://localhost:@PORT/filler");
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, [
      'fdffile' =>  new \CurlFile(__DIR__.'/fdffile.xfdf','text/plain','file.xfdf'),
      'pdf-to-fill' =>  new \CurlFile(__DIR__.'/pdf-to-fill.pdf','application/pdf','my.pdf')
  ]);
  $result = curl_exec($ch);
  file_put_contents("filledpdf.pdf", $result);
```  
# Build and Deploy

