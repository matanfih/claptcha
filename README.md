# Claptcha

A simple CAPTCHA image generator.

This module provides a single class (Claptcha) that can create on the fly
PIL Image instances, BytesIO objects or save image files containing a simple
CAPTCHA strings. Its built on top of Pillow package.

It is required that user provides a TTF file with font to be used in images
and either a string with CAPTCHA text or a callable object returning strings
to be used in images.

## Installation

```bash
$ pip3 install claptcha
```
or
```bash
$ python3 setup.py install
```

## Usage

```python
from claptcha import Claptcha

# Initialize Claptcha object with "Text" as text and FreeMono as font
c = Claptcha("Text", "FreeMono.ttf")

# Get PIL Image object
text, image = c.image

print(text)         # 'Text'
print(type(image))  # <class 'PIL.Image.Image'>

# Get BytesIO object (note that it will represent a different image, just
# with the same text)
text, bytes = c.bytes

print(text)         # 'Text'
print(type(bytes))  # <class '_io.BytesIO'>

# Save a PNG file 'test.png'
text, file = c.write('test.png')

print(text)         # 'Text'
print(file)         # 'test.png'
```

```python
import random
import string
from PIL import Image
from claptcha import Claptcha

def randomString():
    rndLetters = (random.choice(string.ascii_uppercase) for _ in range(6))
    return "".join(rndLetters)

# Initialize Claptcha object with random text, FreeMono as font, of size
# 100x30px, using bicubic resampling filter and adding a bit of white noise
c = Claptcha(randomString, "FreeMono.ttf", (100,30),
             resample=Image.BICUBIC, noise=0.3)

text, _ = c.write('captcha1.png')
print(text)  # 'PZTBXB', string printed into captcha1.png

text, _ = c.write('captcha2.png')
print(text)  # 'NEDKEM', string printed into captcha2.png

# Change images' size to 150x90 and estimated margin to 25x25
c.size = (150,90)
c.margin = (25,25)

text, _ = c.write('captcha3.png')
print(text)  # 'XCQYVS', captcha3.png has different dimentions than
             # captcha1.png and captcha2.png
```

## License

MIT, see `LICENCE`.
