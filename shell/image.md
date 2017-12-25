# Image

## PNG

### Compression

```
pngquant --quality 60-80 -f products-new-0*.png
```

in place:

```
pngquant --quality 60-80 --ext .png -f products-new-0*.png
```

### Convert PDF to PNG

Image magick and graphics magick do pretty much the same thing.

```
$ convert -density 150 input.pdf -quality 90 output.png
```

## SVG


### Compression

+ <https://css-tricks.com/probably-dont-base64-svg/>

```
# npm install -g svgo
$ svgo -i input.svg -o output.svg
```

### URI Encoding

When including in CSS, you need to uri encode

```
$ svgo -i input.svg -o output.svg --datauri enc
```

Simplest way to insert into your CSS is to pipe it to a clipboard utility, such as xsel.

```
$ svgo -i input.svg -o - --datauri enc | xsel -b
```

```
background: url('data:image/svg+xml,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2224%22%20height%3D%2220%22%20viewBox%3D%220%200%2024%2020%22%3E%3Cpath%20d%3D%22M0%200v20h14v-2H2V2h12V0H0zm18%207.408L20.963%2010%2018%2012.592V11h-8V9h8V7.408zM16%203v4H8v6h8v4l8-7-8-7z%22%20fill%3D%22%23fff%22%2F%3E%3C%2Fsvg%3E') 0 0 / 100% auto no-repeat;
```

Never Base64!  SVG is already text based. Converting to base64 increases size (by around 30%).

